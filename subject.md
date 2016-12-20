#
Subject主题
什么是Subject？Subject是允许值被多播到多个观察者的一种特殊的Observable。然而纯粹的可观察对象是单播的(每一个订阅的观察者拥有单独的可观察对象的执行)。
>Subject就是一个可观察对象，只不过可以被多播至多个观察者。同时Subject也类似于EventEmitter:维护者着众多事件监听器的注册表。

**每一个Subject都是一个observable可观察对象**，给定一个Subject后，你可以订阅它，提供的观察者将会正常的开始接收值。从观察者的角度来看，它不能判断一个可观察对象的执行时来自于单播的Observable还是来自于一个Subject.

在Subject的内部，subscribe并不调用一个新的发送值得执行。它仅仅在观察者注册表中注册给定的观察者，类似于其他库或者语言中的addlistener的工作方式。

**每一个Subject都是一个Observer观察者对象**。它是一个拥有next()/error()/complete()方法的对象。要想Subject提供一个新的值，只需调用next()，它将会被多播至用来监听Subject的观察者。

下面的例子，Subject有两个观察者，
```
var subject = new Rx.Subject();

subject.subscribe({
next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
next: (v) => console.log('observerB: ' + v)
});

subject.next(1);
subject.next(2);
```
输出如下:

```
observerA: 1
observerB: 1
observerA: 2
observerB: 2
```
由于Subject也是一个观察者，这就意味着你可以提供一个Subject当做observable.subscribe()的参数，如下:
```
var subject = new Rx.Subject();

subject.subscribe({
next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
next: (v) => console.log('observerB: ' + v)
});

var observable = Rx.Observable.from([1, 2, 3]);

observable.subscribe(subject); // You can subscribe providing a Subject
```
输出如下:


```
observerA: 1
observerB: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3
```

用上面的方式，我们本质上是通过将一个单播的可观察对象转化为多播。这个演示了Subjects是任何将可观察对象执行分享给多个观察者。(译者注:注意观察上面的两个subject.subscribe()中传入的两个观察者对象)
###多播的可观察对象
一个"多播的可观察对象"通过具有多个订阅者的Subject对象传递通知。然而一个单纯的"单播可观察对象"仅仅给一个单一的观察者发送通知。

>一个多播的可观察对象使用一个Subject，使得多个观察者可以看到同一个可观察对象的执行。

```
var source=Rx.Observable.from([1,2,3]);
var subject=new Rx.Subject();
var multicasted=source.multicasted(subject);
multicasted.subscribe({
next:(v)=>console.log('observerS:' +v);
});
multicasted.subscribe({
next: (v) => console.log('observerB: ' + v)
});
multicasted.connect();
```
multicast方法返回一个看起来很像普通的可观察对象的可观察对象，但是在订阅时却有着和Subject一样的行为，multicast返回一个**ConnectableObservable**，它只是一个具有connect（）方法的Observable。

connect()方法对于在决定何时开始分享可观察对象的执行是非常重要的。 因为connect（）在source下面有source.subscribe（subject），connect（）返回一个Subscription，你可以取消订阅，以取消共享的Observable执行。


####reference counting引用计数
调用connect()手动的处理Subscription是很麻烦的，我们想要在第一个观察者到达时自动的链接，并且在最后一个观察者取消订阅后自动的取消可观察对象的执行。
考虑下面的例子，其中按照此列表概述的方式进行订阅。

1.第一个观察者订阅多播可观察对象

2.**多播可观察对象被连接**

3.next value 0 被发送给第一个观察者

4.第二个观察者订阅多播可观察对象

5.next value 1 被发送给第一个观察者

6.next value 1 被发送给第二个观察者

7.第一个观察者取消订阅多播可观察对象

8.next value 1 被发送给第二个观察者

9.第二个观察者取消订阅多播可观察对象

10.**到多播可观察对象的连接被取消**

我们通过显示的调用connect()来实现，如下:
```
var soource = Rx.Observerable.interval(500);
var subject = new Rx.Subject();
var multicasted = source.multicase(subject);
subscription1 = multicasted.subscribe({
next: (v) => console.log('observerA: ' + v)
});
// We should call `connect()` here, because the first
// subscriber to `multicasted` is interested in consuming values
subscriptionConnect = multicasted.connect();
setTimeout(() => {
subscription2 = multicasted.subscribe({
next: (v) => console.log('observerB: ' + v)
});
}, 600);
setTimeout(() => {
subscription1.unsubscribe();
}, 1200);
// We should unsubscribe the shared Observable execution here,
// because `multicasted` would have no more subscribers after this
setTimeout(() => {
subscription2.unsubscribe();
subscriptionConnect.unsubscribe(); // for the shared Observable execution
}, 2000);
```
如果我们希望避免显式的调用connect()，我们可以使用ConnectableObservable对象的refCount()方法(引用计数)，它返回一个追踪它自身有多少个订阅者的可观察对象。当订阅者的数量从0增加到1，它将会为我们调用connect()，这将开始分享可观察对象的执行。在当且仅在订阅者的数量降低到0的时候它将会完全取消订阅，停掉更进一步的执行。
>refCount使得多播可观察对象在其第一个观察者开始订阅时自动的开始执行，在其最后一个订阅者取消的时候终止执行

下面的例子
```
var source = Rx.Observable.intervale(500);
var subject = new Rx.Subject();
var refCounted = source.multicast(subject).refCount();
var subscription1,subscription2,subscriptionConnect;
// This calls `connect()`, because
// it is the first subscriber to `refCounted`
console.log('observerA subscribed');
subscription1 = refCounted.subscribe({
next: (v) => console.log('observerA: ' + v)
});

setTimeout(() => {
console.log('observerB subscribed');
subscription2 = refCounted.subscribe({
next: (v) => console.log('observerB: ' + v)
});
}, 600);

setTimeout(() => {
console.log('observerA unsubscribed');
subscription1.unsubscribe();
}, 1200);

// This is when the shared Observable execution will stop, because
// `refCounted` would have no more subscribers after this
setTimeout(() => {
console.log('observerB unsubscribed');
subscription2.unsubscribe();
}, 2000);
```
上面的例子得到如下的输出:
```
observerA subscribed
observerA: 0
observerB subscribed
observerA: 1
observerB: 1
observerA unsubscribed
observerB: 2
observerB unsubscribed
```
引用计数方法refCount()仅存在于ConnectableObservable，并且它返回一个Obsevable,而不是另一个ConnectableObservable。
###BehaviorSubject
Subjects的一个变体是BehaviorSubject,其有"当前值"的概念。它**储存着**要发射给消费者的最新的值。**无论何时**一个新的观察者订阅它，都会立即接受到这个来自BehaviorSubject的"当前值"。
>BehaviorSubject对于表示"随时间的值"是很有用的。举个例子，人的生日的事件流是一个Subject,然而人的年龄的流是一个BehaviorSubject。

在下面的例子中，BehaviorSubject被数值0初始化，第一个观察者将会在订阅的时候收到这个值。第二个观察者接收数值2，即使它是在数值2被发送之后订阅的。
```
var subject = new Rx.BehaviorSubject(0); // 0 is the initial value

subject.subscribe({
next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);

subject.subscribe({
next: (v) => console.log('observerB: ' + v)
});

subject.next(3);
```
输出如下:
```
observerA: 0
observerA: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3
```
###ReplaySubject 复演主题(自己觉得这样翻译比较好理解，从新发送过去的值)
一个ReplaySubject类似于一个BehaviorSubject，因为它可以发送一个过去的值(old values)给一个新的订阅者，但是它也可以记录可观察对象的**一部分**执行。
>一个ReplaySubject 从一个可观察对象的执行中记录多个值，并且可以重新发送给新的订阅者。

当创建一个ReplaySubject，你可指定有多少值需要重发。
```
var subject = new Rx.ReplaySubject(3); // buffer 3 values for new subscribers ，注:缓存了三个值。

subject.subscribe({
next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
next: (v) => console.log('observerB: ' + v)
});

subject.next(5);
```
输出如下

```
observerA: 1
observerA: 2
observerA: 3
observerA: 4
observerB: 2
observerB: 3
observerB: 4
observerA: 5
observerB: 5
```
除了缓存值得个数之外，你也可以指定一个以毫秒为单位的时间，来决定过去多久出现的值可以被重发。在下面的例子中指定一百个缓存值，但是时间参数仅为500ms。
```
var subject = new Rx.ReplaySubject(100, 500 /* windowTime */);

subject.subscribe({
next: (v) => console.log('observerA: ' + v)
});

var i = 1;
setInterval(() => subject.next(i++), 200);

setTimeout(() => {
subject.subscribe({
next: (v) => console.log('observerB: ' + v)
});
}, 1000);
```
输出如下:
```
observerA: 1
observerA: 2
observerA: 3
observerA: 4
observerA: 5
observerB: 3
observerB: 4
observerB: 5
observerA: 6
observerB: 6
...
```
###AsyncSubject
AsyncSubject是另一个变体，它只发送给观察者可观察对象执行的最新值，并且仅在执行结束时。

```
var subject = new Rx.AsyncSubject();

subject.subscribe({
next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
next: (v) => console.log('observerB: ' + v)
});

subject.next(5);
subject.complete();
```
输出:
```
observerA: 5
observerB: 5
```
AsyncSubject类似于last()操作符,因为它为了发送单一值而等待complete通知。








