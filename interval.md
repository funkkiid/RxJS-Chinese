### interval

- 语法:

```ts
public static interval(period: number, scheduler: Scheduler): Observable
```

- 功能:
返回一个以周期性的、递增的方式发射值的Observable


![](/assets/rxjs.png)
>interval返回一个Observable，它发出一个递增的无限整数序列。第一个参数为时间间隔。 需要注意的是，第一发射不立即发送，而是在第一个周期过去之后发送。 第二个参数，默认情况下，interval使用异步调度程序提供时间概念，但可以将任何调度程序传递给它。

eg:

```js
var numbers = Rx.Observable.interval(1000);
numbers.subscribe(x => console.log(x));
```

f-eg:

```js
var source = Rx.Observable
    .interval(500 /* ms */)
    .timeInterval()
    .take(3);

var subscription = source.subscribe(
    function (x) {
        console.log('Next:', x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: {value: 0, interval: 500}
// => Next: {value: 1, interval: 500}
// => Next: {value: 2, interval: 500} 
// => Completed
```

