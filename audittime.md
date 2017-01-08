###auditTime


语法:
```
public auditTime(durationTime: number, [scheduler: Scheduler]):Observable
```

功能：
在某个时间段内，忽略原始observable发射的值，该时间段由设定的duration的值(单位为ms)来决定，每隔一个设定的时间段，将从原始的observable发射最近的值。不断重复这个过程。



![](/assets/auditTime.png)

eg:
```
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.auditTime(1000);
result.subscribe(x => console.log(x));
//每秒最多出现一次有效点击
```