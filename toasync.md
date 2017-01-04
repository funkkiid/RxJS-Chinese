###toAsync

语法：
```
Rx.Observable.toAsync(function:Function,[scheduler],[context]):Function
```

功能：
将函数转换为异步函数。 生成的异步函数的每次调用都会导致调用指定调度程序上的原始同步函数。
```
var func = Rx.Observable.toAsync(function (x, y) {
    return x + y;
});

// Execute function with 3 and 4
var source = func(3, 4);

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

// => Next: 7
// => Completed
```

注意：该操作符返回的不是observable，而是一个异步函数，当异步函数被调用后(注意，调用**并未立即执行**)，返回一个observable，该observable被订阅时，原函数才会被执行，并返回值。