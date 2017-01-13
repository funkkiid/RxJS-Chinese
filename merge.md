### merge

- 语法：

```ts
public static merage(observable:...Observable,conurrent:number,scheduler:Schedule):Observable
```
- 功能：创建一个发射所有被合并的observable所发射的值。

> 通过将多个观察者的值合并到一个观察者中进行发射。

![](/assets/merge.png)

输出Observable只有在所有输入Observable完成后才会完成。 由输入Observable传递的任何错误将立即在输出Observable上发出。

eg:
```js
var clicks = Rx.Observable.fromEvent(document, 'click');
var timer = Rx.Observable.interval(1000);
var clicksOrTimer = Rx.Observable.merge(clicks, timer);
clicksOrTimer.subscribe(x => console.log(x));

```

```js
//Merge together 3 Observables, but only 2 run concurrently

var timer1 = Rx.Observable.interval(1000).take(10);
var timer2 = Rx.Observable.interval(2000).take(6);
var timer3 = Rx.Observable.interval(500).take(10);
var concurrent = 2; // the argument
var merged = Rx.Observable.merge(timer1, timer2, timer3, concurrent);
merged.subscribe(x => console.log(x));
```


f-eg:

```js
var source1 = Rx.Observable.interval(100)
    .timeInterval()
    .pluck('interval');
var source2 = Rx.Observable.interval(150)
    .timeInterval()
    .pluck('interval');

var source = Rx.Observable.merge(
    source1,
    source2).take(10);


var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 100
// => Next: 150
// => Next: 100
// => Next: 150
// => Next: 100 
// => Completed
```
