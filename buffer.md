###buffer

语法：
```
public buffer(closingNotifier: Observable):Observable<T[]>
```
功能：缓存原始observable发射的值，直到作为参数的另一个observable发射了值。之后返回一个由这些缓存值组成的数组。

![](/assets/buffer.png)


eg:
```
var clicks = Rx.Observable.fromEvent(document, 'click');
var interval = Rx.Observable.interval(1000);
var buffered = interval.buffer(clicks);
buffered.subscribe(x => console.log(x));
```

eg-result
![](/assets/buffer-result.png)