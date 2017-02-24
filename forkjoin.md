### forkJoin

- 语法:

```ts
Rx.Observable.forkJoin(...args [resultSelector])
```

- 功能：
并行运行所有可观察序列并收集其最后的元素。

f-eg:

```js
/* Using observables and Promises */
var source = Rx.Observable.forkJoin(
    Rx.Observable.return(42),
    Rx.Observable.range(0, 10),
    Rx.Observable.fromArray([1,2,3]),
    RSVP.Promise.resolve(56)
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => Next: [42, 9, 3, 56]
// => Completed
```

