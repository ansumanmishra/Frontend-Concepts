# Higher order observables

Higher-order observables are observables that emit other observables, rather than just simple data values. In other words, they are observables of observables. Higher-order observables often arise when you use certain RxJS operators that return observables, such as flatMap, mergeMap, switchMap, and concatMap. 

For avoiding nested subscriptions, we use higher order observables

## 1. mergeMap
In this example, we have a source observable emitting 1, 2, and 3. We use mergeMap to simulate concurrent API requests using fakeRequest. Each request takes 1 second to complete, and the output is logged as soon as each request finishes, without maintaining any specific order. If 2nd api request takes more time then 3rd. Then it will print the result of 3rd request and then 2nd one.

```javascript
import { of } from 'rxjs';
import { mergeMap, delay } from 'rxjs/operators';

const source = of(1, 2, 3);
const fakeRequest = val => of(`Request ${val}`).pipe(delay(1000));

source.pipe(
  mergeMap(val => fakeRequest(val))
).subscribe(console.log);

// Output (order may vary due to concurrency):
// Request 1
// Request 2
// Request 3
```

Example use case: Multiple parallel API requests where the order of responses doesn't matter like search, get requests etc.

## 2. concatMap
Here the output is logged in the order of the requests, with each request taking 1 second to complete. It maintains the order.

```javascript
import { of } from 'rxjs';
import { concatMap, delay } from 'rxjs/operators';

const source = of(1, 2, 3);
const fakeRequest = val => of(`Request ${val}`).pipe(delay(1000));

source.pipe(
  concatMap(val => fakeRequest(val))
).subscribe(console.log);

// Output (order maintained):
// Request 1
// Request 2
// Request 3
```
Example use case: A sequence of API requests where the order of responses matters or depends on previous responses (e.g., saving a record and then fetching an updated list).

## 3. exhaustMap
In this example, we have a source observable emitting 1, 2, and 3. We use exhaustMap to process a fakeRequest for each emitted value. Each request takes 2 seconds to complete, but since exhaustMap ignores overlapping inner observables, only the first request is processed and logged. The values 2 and 3 are ignored because they are emitted while the first request is still in progress.

```javascript
import { of, timer } from 'rxjs';
import { exhaustMap, delay } from 'rxjs/operators';

const source = of(1, 2, 3);
const fakeRequest = val => of(`Request ${val}`).pipe(delay(2000));

source.pipe(
  exhaustMap(val => fakeRequest(val))
).subscribe(console.log);

// Output (ignores overlapping values):
// Request 1
```
Example use case: Preventing repeated form submissions by ignoring multiple clicks on a submit button until the initial request completes.