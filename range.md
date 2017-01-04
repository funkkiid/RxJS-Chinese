###range

语法：
```
public static range(start:number,count:number,scheduler:Scheduler):Observable
```

功能：创建发射一个数字序列的observable

![](/assets/rangge.png)


range按顺序发出一系列连续整数，参数分别为起点和长度(注意不是终点)。 默认情况下，不使用调度程序，只是**同步**传递通知，但可以使用可选的调度程序来调节这些传递。


eg:

```
var numbers = Rx.Observable.range(1, 10);
numbers.subscribe(x => console.log(x));
```
result:
![](/assets/range-result.png)


f-eg:
```
var source = Rx.Observable.range(0, 3);

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

// => Next: 0 
// => Next: 1
// => Next: 2 
// => Completed 
```