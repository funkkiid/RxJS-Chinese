###audit

语法：
```
puiblic audit(durationSelector:function(value:T):Observable):Observable<T>
```

功能：

在某个**持续时间段**内忽略原始observable发射的值
，该方法的参数为一个**函数**，该函数需返回一个决定持续时长
的observable或者promise。之后从原始observable发射最近的值，不断重复这个过程。

>audit很像auditTime，但是其持续时长是由第二个observable所决定。

![](/assets/audit.png)


eg:
```
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.audit(ev => Rx.Observable.interval(1000));
result.subscribe(x => console.log(x));
//每秒只会有一次单击会被发射，发射的时间点为每隔1秒
```
