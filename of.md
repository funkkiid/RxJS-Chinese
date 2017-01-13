### of

语法：

```ts
public static of (values:...T,scheduler:Scheduler):Observable
```

- 功能：
创建一个Observable，发射指定参数的值，一个接一个，最后发出complate。

![](/assets/complate.png)
of用于创建一个简单的Observable，只发出给定的参数，然后发出完整的通知。 它可以用于与其他Observable组合，如concat。 默认情况下，它使用一个空的调度程序，这意味着next通知是同步发送(看下面的例子)，虽然使用不同的调度程序，可以确定这些通知何时将被交付。

eg:
```js
// Emit 1, 2, 3, then 'a', 'b', 'c', then start ticking every second.
var numbers = Rx.Observable.of(1, 2, 3);
var letters = Rx.Observable.of('a', 'b', 'c');
var interval = Rx.Observable.interval(1000);
var result = numbers.concat(letters).concat(interval);
result.subscribe(x => console.log(x));
```
![](/assets/of-result.png)


f-eg:
```js
var source = Rx.Observable.of(1,2,3);

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

// => Next: 1
// => Next: 2
// => Next: 3
// => Completed
```