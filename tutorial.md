##The basics基础
####Converting to observables  转换为可观察对象
```
//From one or multiple values  一个或多个值->可观察对象
Rx.observable.of('foo','bar');
```
```
//From array of values         数组->可观察对象
Rx.Observable.from([1,2,3]);
```
```
//From an event                事件->可观察对象
Rx.Observable.fromEvent(document.querySelector('button'),'click');
注:arg1为DOM对象，arg2为事件类型
```

```
//From a promise               promise->可观察对象
Rx.Observable.fromPromise(fetch('/users'));
```
```
//From a callback(last argument is a callback)  回调函数->可观察对象
//fs.exists = (path,cb(exists))
var exists = Rx.Observable.bindCallback(fs.exists);
exists('file.txt').subscribe(exists => console.log('Does file exist?', exists));
```
```
// From a callback (last argument is a callback)
// fs.rename = (pathA, pathB, cb(err, result))
var rename = Rx.Observable.bindNodeCallback(fs.rename);
rename('file.txt', 'else.txt').subscribe(() => console.log('Renamed!'));
```

####Creating observables  创建可观察对象
外部产生新事件
```
var myObservable = new Rx.Subject();
myObservable.subscribe(value => console.log(value));
myObservable.next('foo');
```
内部产生新事件
```
var myObservable = Rx.Observable.create(observer => {
  observer.next('foo');
  setTimeout(() => observer.next('bar'), 1000);
});
myObservable.subscribe(value => console.log(value));
```
####Controlling the flow 控制流
```
// typing "hello world"
var input = Rx.Observable.fromEvent(document.querySelector('input'), 'keypress');

// Filter out target values less than 3 characters long
input.filter(event => event.target.value.length > 2)
  .subscribe(value => console.log(value)); // "hel"

// Delay the events
input.delay(200)
  .subscribe(value => console.log(value)); // "h" -200ms-> "e" -200ms-> "l" ...

// Only let through an event every 200 ms
input.throttleTime(200)
  .subscribe(value => console.log(value)); // "h" -200ms-> "w"

// Let through latest event after 200 ms
input.debounceTime(200)
  .subscribe(value => console.log(value)); // "o" -200ms-> "d"

// Stop the stream of events after 3 events
input.take(3)
  .subscribe(value => console.log(value)); // "hel"

// Passes through events until other observable triggers an event
var stopStream = Rx.Observable.fromEvent(document.querySelector('button'), 'click');
input.takeUntil(stopStream)
  .subscribe(value => console.log(value)); // "hello" (click)
```
####producing values 生产值
```
// typing "hello world"
var input = Rx.Observable.fromEvent(document.querySelector('input'), 'keypress');

// Pass on a new value
input.map(event => event.target.value)
  .subscribe(value => console.log(value)); // "h"

// Pass on a new value by plucking it
input.pluck('target', 'value')
  .subscribe(value => console.log(value)); // "h"

// Pass the two previous values
input.pluck('target', 'value').pairwise()
  .subscribe(value => console.log(value)); // ["h", "e"]

// Only pass unique values through
input.pluck('target', 'value').distinct()
  .subscribe(value => console.log(value)); // "helo wrd"

// Do not pass repeating values through
input.pluck('target', 'value').distinctUntilChanged()
  .subscribe(value => console.log(value)); // "helo world"

```
####Creating applications   创建应用程序
RxJS是一个能够使得你的代码保持更小的错误倾向的强大的工具。这源自于它使用无状态的纯函数。但是应用程序是有状态的，那么我们是如何将RxJS的无状态世界里与应用程序的有状态联系起来的呢？让我们看一个简单的例子:对数值0的状态存储。在每一次点击我们想增加在状态存储中的计数。
```
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  // scan (reduce) to a stream of counts
  .scan(count => count + 1, 0)
  // Set the count on an element each time it changes
  .subscribe(count => document.querySelector('#count').innerHTML = count);
```
可以看出，状态是在RxJS中产生的，但是最后一行中改变DOM是一个副作用。



