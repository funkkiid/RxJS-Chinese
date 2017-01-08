###bufferToggle

语法:
```
public bufferToggle():Observable<T[]>
```

![](/assets/bufferWhen.png)

```
//每隔1~5秒发射一次最新的click事件数组
var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferWhen(() =>
  Rx.Observable.interval(1000 + Math.random() * 4000)
);
buffered.subscribe(x => console.log(x));
```