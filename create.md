###create


语法:
```
public static create(subscribe: function(subscriber: Subscriber): TeardownLogic): Observable
```

功能:

创建一个新的Observable，当被订阅时，它将执行指定的函数。

>创建一个拥有在订阅函数中给定逻辑的可观察对象

![](/assets/a3.png)

create将subscribe函数转换为实际的Observable。 这相当于调用Observable构造函数。 编写subscribe函数，使其作为一个Observable：它应该调用订阅者的next，error和complate方法,遵循Observable约束；良好的Observable必须调用Subscriber的complate方法一次或其error方法一次，然后再不会调用之后的next。

大多数时候，您不需要使用create，因为现有的创建操作符（以及实例组合运算符）允许您为大多数用例创建一个Observable。 但是，create是低级的，并且能够创建任何Observable。


eg:
```
var result = Rx.Observable.create(function (subscriber) {
  subscriber.next(Math.random());
  subscriber.next(Math.random());
  subscriber.next(Math.random());
  subscriber.complete();
});
result.subscribe(x => console.log(x));
```
