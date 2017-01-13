### concat

- 语法:

```ts
public static concat(input1: Observable, input2: Observable, scheduler: Scheduler): Observable
```

- 功能:
产生一个Observable，它取作为参数的Observable发射的每个值，并顺序(基于作为输入的可观察对象的顺序)发出所有值。

> 连接多个可观察对象，并顺序发出他们的值，一个可观察对象跟在另一个的后面

![](/assets/a2.png)


eg1：
```js
// Concatenate a timer counting from 0 to 3 with a synchronous sequence from 1 to 10
var timer = Rx.Observable.interval(1000).take(4);
var sequence = Rx.Observable.range(1, 10);
var result = Rx.Observable.concat(timer, sequence);
result.subscribe(x => console.log(x));
```

eg2：
```js
var timer1 = Rx.Observable.interval(1000).take(10);
var timer2 = Rx.Observable.interval(2000).take(6);
var timer3 = Rx.Observable.interval(500).take(10);
var result = Rx.Observable.concat(timer1, timer2, timer3);
result.subscribe(x => console.log(x));
```
