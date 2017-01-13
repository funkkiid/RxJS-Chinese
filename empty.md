### empty

- 语法:

```ts
public static empty(scheduler: Scheduler): Observable
```

- 功能:
创建一个不发射任何值的Observable,它只会发射一个complate通知。

> 仅仅发射一个‘complate’,除此之外，不会发射其他值。

![](/assets/a5.png)

该操作符创建一个仅发射‘complete’的通知。通常用于和其他操作符一起组合使用。

eg:



```js
//Emit the number 7, then complete.
var result = Rx.Observable.empty().startWith(7);
result.subscribe(x => console.log(x));
```

```js
//Map and flatten only odd numbers to the sequence 'a', 'b', 'c
var interval = Rx.Observable.interval(1000);
var result = interval.mergeMap(x =>
  x % 2 === 1 ? Rx.Observable.of('a', 'b', 'c') : Rx.Observable.empty()
);
result.subscribe(x => console.log(x));
```
f-eg:

```js
var source = Rx.Observable.empty();

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onCompleted
```



