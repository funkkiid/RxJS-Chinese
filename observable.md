#observable可观察对象
可观察对象以惰性的方式推送多值的集合。


|Single单值|Multiple多值|
-|
pull拉|Function|Iterator
push推|Promise|Observable
下面的例子是一个推送1,2，3,4数值的可观察对象，一旦它被订阅1,2，3,就会被推送，4则会在订阅发生一秒之后被推送，紧接着完成推送。
```
var observable = Rx.Observable.create(function (observer) {
observer.next(1);
observer.next(2);
observer.next(3);
setTimeout(() => {
observer.next(4);
observer.complete();
}, 1000);
});
```
调用可观察对象然后得到它所推送的值，我们订阅它，如下
```
console.log('just before subscribe');
observable.subscribe({
next: x => console.log('got value ' + x),
error: err => console.error('something wrong occurred: ' + err),
complete: () => console.log('done'),
});
console.log('just after subscribe');
```
结果如下
```
just before subscribe
got value 1
got value 2
got value 3
just after subscribe
got value 4
done
```
###Pull拉取VSPush推送
拉和推是数据生产者和数据的消费者两种不同的交流协议(方式)

什么是"_Pull拉_"？在"拉"体系中，数据的消费者决定何时从数据生产者那里获取数据，而生产者自身并不会意识到什么时候数据将会被发送给消费者。

每一个JS函数都是一个“拉”体系，函数是数据的生产者，调用函数的代码通过“拉出”一个单一的返回值来消费该数据(return 语句)。

ES6介绍了[iterator迭代器](http://es6.ruanyifeng.com/#docs/iterator)和[Generator生成器](http://es6.ruanyifeng.com/#docs/generator)——另一中“拉”体系，调用iterator.next()的代码是消费者，可从中拉取_多个值_。

|producer|Consumer|
-|
pull拉|Passive(被动的一方):被请求的时候产生数据|Active(起主导的一方):决定何时请求数据
push推|Active:按自己的节奏生产数据|Passive:对接收的数据做出反应(处理接收到的数据)

什么是"Push推"？在推体系中，数据的生产者决定何时发送数据给消费者，消费者不会在接收数据之前意识到它将要接收这个数据。

[Promise(承诺)](http://es6.ruanyifeng.com/#docs/promise))是当今JS中最常见的Push推体系，一个Promise(数据的生产者)发送一个resolved value(成功状态的值)来注册一个回调(数据消费者)，但是不同于函数的地方的是：Promise决定着何时数据才被推送至这个回调函数。


RxJS引入了Observables(可观察对象)，一个全新的"推体系"。一个可观察对象是一个产生多值的生产者，并"推送给"Observer(观察者)。
* Function:只在调用时惰性的计算后同步地返回一个值
* Generator(生成器):惰性计算，在迭代时同步的返回零到无限个值(如果有可能的话)
* Promise是一个可能(也可能不)返回一个单值的计算。
* Observable是一个从它被调用开始，可异步或者同步的返回零到多个值的惰性执行运算。




###可观察对象——作为更一般化的函数
与常见的主张相悖的是，可观察对象不像EventEmitters(事件驱动)，也不象Promises因为它可以返回多个值。可观察对象可能会在某些情况下有点像EventEmitters(事件驱动)，也即是当它们使用Subjects被多播时，但是大多数情况下，并不像EventEmitters.

> 可观察对象像一个零参的函数，但是允许返回多个值使得其更加的一般化。

思考下面的程序

```
function foo() {
console.log('Hello');
return 42;
}

var x = foo.call(); // same as foo()
console.log(x);
var y = foo.call(); // same as foo()
console.log(y);
```

我们可以看到这样的输出。

```
"Hello"
42
"Hello"
42

```

使用Observables得到同样的结果

```
var foo=Rx.Observable.create(function(observer){
console.log('Hello');
observer.next(42);
});

foo.subscribe(function(x){
console.log(x)；
});
foo.subscribe(function (y){
console.log(y);
});
```
得到同样的输出


```
"Hello" 42 "Hello" 42
```


这种情况源自于函数和可观察对象均是惰性计算。如果你不'call(调用)'函数,console.log('Hello')将不会发生。可观察对象同样如此，如果你不"调用"(使用subscribe，订阅)，console.log('Hello')也将不会发生。此外，'calling'或者'subscribing'是一个独立的操作:两次函数调用触发两个独立副作用，两次订阅触发两个独立的副作用。相反的，EventEmitters:共享副作用并且不管订阅者的存在而去执行。
>订阅一个可观察对象类似于调用一个函数。

一些人认为可观察对象是异步的。这并不确切，如果你用一些log语句包围在订阅程序的前后:


```
console.log('before');
console.log(foo.call());
console.log('after');
```
你可以得到如下结果
"

```
before"
"Hello"
42
"after"
```
类似的，使用可观察对象:


```
console.log('before');
foo.subscribe(function (x) {
console.log(x);
});
console.log('after');

```
输出如下:

```
"before"
"Hello"
42
"after"
```
以上可以显示对foo的订阅是完全同步的，就像调用一个函数。
>可观察对象u以同步或者异步的方式发送多个值。

好吧，调转方向，说一说可观察对象和函数的不同之处。可观察对象可以随时间"return"多个值。然而函数却做不到，你不能够使得如下的情况发生:

```
function foo() {
console.log('Hello');
return 42;
return 100; // dead code. will never happen
}
```
函数仅仅可以返回一个值，然而，不要惊讶，可观察对象却可以做到这些。




```
var foo = Rx.Observable.create(function (observer) {
console.log('Hello');
observer.next(42);
observer.next(100); // "return" another value
observer.next(200); // "return" yet another
});

console.log('before');
foo.subscribe(function (x) {
console.log(x);
});
console.log('after');
```
同步输出:

```
"before"
"Hello"
42
100
200
"after"
```
当然，你也可以以异步的方式返回值。


```
var foo = Rx.Observable.create(function (observer) {
console.log('Hello');
observer.next(42);
observer.next(100);
observer.next(200);
setTimeout(() => {
observer.next(300); // happens asynchronously
}, 1000);
});

console.log('before');
foo.subscribe(function (x) {
console.log(x);
});
console.log('after');
```
输出
```

"before"
"Hello"
42
100
200
"after"
300
```
总结:
* fun.call()意味着同步的给我**一个**值
* observable.subscribe()意味着给我_**任意多个**_值，同步也好异步也罢。
##剖析可观察对象
使用Rx.Observable.create或者一个能产生可观察对象的操作符来创造一个可观察对象，使用一个观察者订阅它，执行然后给观察者发送next/error/complete通知。他们的执行可能会被disposed(处理)。这四个方面均被编码进可观察对象的实例中。但是其中的某些方面和其他的类型有关，如Observer和Subscription


核心的可观察对象概念(个人觉得这一块还是不翻译的好，原汁原味)
* Creating Observables
* Subscribing to Observables
* Executing the Observable
* Disposing Observables

###Creating Observables
Rx.Observable.create 是可观察对象构造函数的别名，它接受一个参数:the subscribe function。

下面的例子创造一每秒向观察者发射一个字符串"hi"的可观察对象。



```
var observable = Rx.Observable.create(function subscribe(observer) {
var id = setInterval(() => {
observer.next('hi')
}, 1000);
});
```
>可观察对象可以使用create创建，但是通常我们使用被称为creation operators,像of,from,interval等。

在上面的例子中，subscribe(订阅)函数是订阅可观察对象最重要的部分。接下来让我们看下订阅的含义是什么。
###订阅可观察对象
观察对象可以像下面的例子那样被订阅:


```
observable.subscribe(x => console.log(x));

```
observable.subscribe和Observable.create(function subscribe(observer){})的subcribe回调函数有着同样的名字并不是因缘巧合。在RxJS中，他们是不同的，但是为了更使用的目的，你可以认为他们在概念上是等价的。

这显示出对于同一个可观察对象进行订阅的多个观察者之间的回调函数是不共享信息的。当使用observer调用observable.subscribe时，Observable.create(function subscribe(observer){})中的subscribe函数为既定的observer运行。每次调用observable.subscribe为给定的观察者触发它自身独立的设定程序。
>订阅一个可观察对象就像调用一个函数，在数据将被发送的地方提供回调。

完全不同于诸如addEventListener/removeEventListener事件句柄API.使用observable.subscribe,给定的观察者并没有作为一个监听者被注册。可观察对象甚至也不保存有哪些观察者。

订阅是启动可观察对象执行和发送值或者事件给观察者的简单方式。

###执行可观察对象
在Observable.create(function(observer){...})中的代码，表示了一个可观察对象的执行，一个仅在观察者订阅的时候发生的惰性计算。执行随着时间产生多个值，以同步或者异步的方式。
下面是可观察对象执行可以发送的三种类型的值
* "**Next**": 发送一个数字/字符串/对象等值。
* "**Error**": 发送一个JS错误或者异常。
* "**Complete**" 不发送值。

Next通知是最重要且最常见的类型:它们代表发送给观察者的确切数据，Error和Complete通知可能仅在可观察对象执行期间仅发生一次，但仅会执行二者之中的一个。

这些约束条件能够在可观察对象语法以类似于正则表达式的方式表达的更清晰:
```
next*(error|complete)?
```
>一个可观察对象的执行期间，零个到无穷多个next通知被发送。如果Error或者Complete通知一旦被发送，此后将不再发送任何值。


下面这个例子，可观察对象执行然后发送三个next通知，然后completes:

```
var observable = Rx.Observable.create(function subscribe(observer) {
observer.next(1);
observer.next(2);
observer.next(3);
observer.complete();
});
```
可观察对象严格的坚守这个契约，所以，下面的代码将不会发送包含数值4的next通知


```
var observable = Rx.Observable.create(function subscribe(observer) {
observer.next(1);
observer.next(2);
observer.next(3);
observer.complete();
observer.next(4); // Is not delivered because it would violate the contract
});
```
不失为一个好方式的是，使用try/catch语句包裹通知语句，如果捕获了异常将会发送一个错误通知。

```
var observable = Rx.Observable.create(function subscribe(observer) {
try {
observer.next(1);
observer.next(2);
observer.next(3);
observer.complete();
} catch (err) {
observer.error(err); // delivers an error if it caught one
}
});
```
###处理可观察对象的执行
由于可观察对象的执行可能是无限的(不停地next)，而对于观察者来说却往往希望在有限的时间内终止执行，因此我们需要一个API来取消执行。因为每一次的执行仅仅服务于一个观察者，一旦观察者听得接收数据，它就不得不通过一个方式去终止执行，从而避免浪费大量的计算性能和内存资源。

当observable.subscribe被调用，观察者将专注于最新被创建的可观察对象的执行，并且这个调用返回一个对象:the SUbscription

```
var subscription = observable.subscribe(x => console.log(x));
```

the Subscription(订阅)表示正在进行的执行，这里有一个用于终止执行的小型的API。 阅读更多关于[Subscription](http://reactivex.io/rxjs/manual/overview.html#subscription)的信息。使用subscription.unsubscribe()你可以取消正在进行的执行:


```
var observable = Rx.Observable.from([10, 20, 30]);
var subscription = observable.subscribe(x => console.log(x));
// Later:
subscription.unsubscribe();
```
>在你订阅了之后，你将会得到一个Subscription对象，它表示正在进行的执行。大胆的去使用unsubscribe()去终止执行吧。

当我们使用create()创建可观察对象，每一个可观察对象必须确定怎样去处理该执行的资源。你可以通过在subscribe函数内返回的subscription调用unsubscribe函数做到这一点。

作为示例，下面是怎样去清除一个serInterval间隔执行

```
var observable = Rx.Observable.create(function subscribe(observer) {
// Keep track of the interval resource
var intervalID = setInterval(() => {
observer.next('hi');
}, 1000);

// Provide a way of canceling and disposing the interval resource
return function unsubscribe() {
clearInterval(intervalID);
};
});
```
就像observable.subscribe类似于Observable.create(function subscribe(){...}),我们从subscribe函数中返回的unsubscribe函数在概念上等价于subscription.unsubscription。事实上，如果我们移除环绕于这些概念之外的ReactiveX类型，也就只剩下更加直观的JavaScript。
```
function subscribe(observer) {
var intervalID = setInterval(() => {
observer.next('hi');
}, 1000);

return function unsubscribe() {
clearInterval(intervalID);
};
}

var unsubscribe = subscribe({next: (x) => console.log(x)});

// Later:
unsubscribe(); // dispose the resources
```
我们使用诸如可观察对象，观察者，和订阅等Rx类型的原因是通过操作符能够兼顾安全性(例如Observable Contract)和相似性。


























