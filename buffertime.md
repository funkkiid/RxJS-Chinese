###bufferTime





语法:
```
public bufferTime(bufferTimeSpan:number,[bufferCreationInterval:numbewr],[maxBufferSize:number],[scheduler:Scheduler]):Observable<T[]>
```
params:

arg1: **必须**、bufferTimeSpan设置发射值的时间间隔
arg2: 可选、设置打开缓存区和发射值的时间间隔
arg2: 可选、设置缓存区长度
scheduler: 可选
功能:
把过去的值放入一个缓存区，并且按时间定期发射这些数组

![](/assets/bufferTime.png)

eg-1：
```
//只有第一个参数时
//每间隔一秒发射一次，数据包含在该时间段的值
var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferTime(1000);
buffered.subscribe(x => console.log(x));
```

eg-1-result:
在第二次结束到第三次发射之间我点击了四次

第三次到第四次之间点击了三次
![](/assets/bufferTime-result-1.png)


