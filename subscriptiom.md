# Subscription订阅
什么是订阅？订阅是一个表示一次性资源的对象，通常是一个可观察对象的执行。订阅对象有一个重要的方法:unsubscribe，该方法不需要参数，仅仅去废弃掉可观察对象所持有的资源。在以往的RxJS的版本中，"Subscription订阅"被称为"Disposable"。

```js
var observable = Rx.Observable.interval(1000);
var subscription = observable.subscribe(x => console.log(x));
// Later:
// This cancels the ongoing Observable execution which
// was started by calling subscribe with an Observer.
subscription.unsubscribe();
```
>订阅对象有一个unsubscribe()方法用来释放资源或者取消可观察对象的执行。

订阅对象也可以被放置在一起，因此对一个订阅对象的unsubscribe()进行调用，可以对多个订阅进行取消。做法是:把一个订阅"加"进另一个订阅。
```js
var observable1 = Rx.Observable.interval(400);
var observable2 = Rx.Observable.interval(300);

var subscription = observable1.subscribe(x => console.log('first: ' + x));
var childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
// Unsubscribes BOTH subscription and childSubscription
subscription.unsubscribe();
}, 1000);
```
执行之后，我们可以在控制台得到:
```
second: 0
first: 0
second: 1
first: 1
second: 2
```
订阅也有一个remove(otherSubscription)方法,用于解除被add添加的子订阅。





