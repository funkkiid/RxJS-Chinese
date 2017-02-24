### defer

- 语法:

```ts
public static defer(observableFactory: function(): Observable | Promise): Observable
```
> 以惰性的方式产生一个Observable,也就是说，当被订阅的时候才会产生。

- 功能:
参数为一个Observable工厂函数，当被订阅时工厂函数被调用产生一个可观察对象。
![](/assets/a4.png)

defer允许您仅在Observer订阅时创建Observable，并为每个Observer创建一个新的Observable。 它等待一个Observer订阅它，然后它生成一个Observable，通常有一个Observable工厂函数。 它为每个用户分别产生一个Observable，所以虽然每个用户可能认为它们是订阅的同一个Observable，事实上每个订阅者都有自己的单独的Observable。

eg:


```js
var clicksOrInterval = Rx.Observable.defer(function () {
  if (Math.random() > 0.5) {
    return Rx.Observable.fromEvent(document, 'click');
  } else {
    return Rx.Observable.interval(1000);
  }
});
clicksOrInterval.subscribe(x => console.log(x));
```

f-eg：

```js
/* Using an observable sequence */
var source = Rx.Observable.defer(() => Rx.Observable.return(42));

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```

```js
/* Using a promise */
var source = Rx.Observable.defer(() => RSVP.Promise.resolve(42));

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```



