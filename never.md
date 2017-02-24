### never

- 语法：

```ts
public static never():Observable
```

- 功能：
创建一个不发射任何值的Observable

![](/assets/never.png)

这个静态操作符对需要创建一个不发射next值、error错误、也不发射complate的简单Observable很有用。 它可以用于测试或与其他Observable组合。 请不要说，从不发出一个完整的通知，这个Observable保持订阅不被自动处置。 订阅需要手动处理。

eg:
```js
function info() {
  console.log('Will not be called');
}
var result = Rx.Observable.never().startWith(7);
result.subscribe(x => console.log(x), info, info);
```

f-eg:
```js
// This will never produce a value, hence never calling any of the callbacks
var source = Rx.Observable.never();

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
```