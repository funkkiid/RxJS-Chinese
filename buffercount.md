###bufferCount

语法：
```
public bufferCount(bufferSize: Number,startBufferEvery):Observable<T[]>
```

功能：缓存原始observable发射的值，直到达到bufferSize给定的上限

![](/assets/bufferCount.png)

eg:
```
var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferCount(2);
buffered.subscribe(x => console.log(x));
```

eg-result:
![](/assets/bufferCount-result.png)

每点击两次，发射一次由之前点击事件组成的事件数组