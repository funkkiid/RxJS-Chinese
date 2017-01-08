###bufferToggle

语法:
```
public bufferToggle(openings: SubscribableOrPromise<O>, closingSelector: function(value: O): SubscribableOrPromise): Observable<T[]>
```
功能:
以数组形式收集过去的值。 在opening发射值时开始收集，并调用closingSelector函数获取一个Observable，以告知何时关闭缓存区。
![](/assets/bufferToggle.png)

```

var clicks = Rx.Observable.fromEvent(document, 'click');
var openings = Rx.Observable.interval(1000);
var buffered = clicks.bufferToggle(openings, i =>
  i % 2 ? Rx.Observable.interval(500) : Rx.Observable.empty()
);
buffered.subscribe(x => console.log(x));
```