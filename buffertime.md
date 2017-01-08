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

每隔bufferTimeSpan毫秒发送和重置缓冲区。 

在第二次结束到第三次发射之间我点击了四次

第三次到第四次之间点击了三次
![](/assets/bufferTime-result-1.png)

eg-2
```
//有前两个参数时
var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferTime(2000, 5000);
buffered.subscribe(x => console.log(x));
```
eg-2
```
var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferTime(2000, 5000);
buffered.subscribe(x => console.log(x));
```
如果给定了bufferCreationInterval，则此操作符每隔bufferCreationInterval毫秒打开缓冲区，并且每隔bufferTimeSpan毫秒关闭（发射和重置）缓冲区。 当指定可选参数maxBufferSize时，缓冲区将在bufferTimeSpan毫秒后或包含maxBufferSize元素时关闭。


**注意**

文档说'每隔bufferTimeSpan' 发射一次，但是我每次都是每隔bufferCreationInterval发射值，原因可能是文档出错或者我理解有误。该问题已经提交为一个issue，之后会更新。