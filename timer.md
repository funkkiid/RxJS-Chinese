###timer

eg

语法：

```
public static timer(initialDelay:number|Date,period:number,schedulaer):Observable
```
>类似于interval,但是第一个参数用来设置发射第一个值得延迟时间



![](/assets/timer.png)

timer返回一个Observable，发出一个无限的上升整数序列，第二个参数为时间的间隔。 第一次发射发生在指定的延迟之后。 初始延迟可以是日期。 默认情况下，此运算符使用**异步**Schuduler提供时间概念，但可以将**任何**schuduler传递给它。 如果未指定period，则输出Observable仅发出一个值0，否则将发出无限序列。

eg:
```
var numbers = Rx.Observable.timer(3000, 1000);
numbers.subscribe(x => console.log(x));
```
eg-result:

![](/assets/timer-result.png)