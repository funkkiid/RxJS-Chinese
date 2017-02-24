### bindCallback()

- 语法:

```ts
public static bindCallback(func: function, selector: function, scheduler: Scheduler): function(...params: *): Observable
```
- 功能:
将一个回调函数API转化为一个能返回一个Observable的函数;

实际上这不是一个严格意义上的操作符，因为无论它的输入还是输出都不是Observable。在输入函数的参数中，最后一个参数为一个回调函数，bindCallback输出的函数接收和inputfn一样的参数(除去最后的callback)，当输出函数被调用时，它将返回一个可观察对象。

```js
// 例子
// Convert jQuery's getJSON to an Observable API
// Suppose we have jQuery.getJSON('/my/url', callback)
var getJSONAsObservable = Rx.Observable.bindCallback(jQuery.getJSON);
var result = getJSONAsObservable('/my/url');
result.subscribe(x => console.log(x), e => console.error(e));
```