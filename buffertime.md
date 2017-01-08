###bufferTime
语法:
```
public bufferTime(bufferTimeSpan:number,[bufferCreationInterval:numbewr],[maxBufferSize:number],[scheduler:Scheduler]):Observable<T[]>
```
params:

arg1: **必须**、bufferTimeSpan设置发射值的时间间隔
arg2: 可选、设置打开缓存区和发射值的时间间隔
arg2: 可选、设置缓存区长度

功能:
把过去的值放入一个缓存区，并且按时间定期发射这些数组

![](/assets/bufferTime.png)

eg