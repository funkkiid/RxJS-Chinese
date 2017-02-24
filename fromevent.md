### fromEvent

- 语法:

```ts
Rx.Observable.fromEvent(element, eventName, [selector])
```

- 功能:
将一个元素上的事件转化为一个Observable


- 注意:
使用jQuery，Zepto，Backbone.Marionette，AngularJS和Ember.js的库方法，并且如果不存在，则回退到本地绑定。 如果您使用AMD，您可能需要将这些库作为RxJ的依赖关系包括在requirejs配置文件中。 当决定使用哪个库时，RxJ将尝试检测它们的存在。

![](/assets/fromEvent.png)


eg：
```js
var clicks = Rx.Observable.fromEvent(document, 'click');
clicks.subscribe(x => console.log(x));
```

f-eg:


```js
var input = $('#input');

var source = Rx.Observable.fromEvent(input, 'click');

var subscription = source.subscribe(
    function (x) {
        console.log('Next: Clicked!');
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

input.trigger('click');

// => Next: Clicked!
```


```js
// nodejs

var EventEmitter = require('events').EventEmitter,
    Rx = require('rx');

var eventEmitter = new EventEmitter();

var source = Rx.Observable.fromEvent(
    eventEmitter,
    'data', 
    function (args) {
        return { foo: args[0], bar: args[1] };
    });

var subscription = source.subscribe(
    function (x) {
        console.log('Next: foo -' x.foo + ', bar -' + x.bar);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

eventEmitter.emit('data', 'baz', 'quux');
// => Next: foo - baz, bar - quux
```




