### bindNodeCallback

- 语法:

```ts
public static bindNodeCallback(func: function, selector: function, scheduler: Scheduler): function(...params: *): Observable
```

- 功能:
将一个NodeJS风格的回调函数API转化为一个能返回可观察对象的函数；

> 基本等同于bindCallback,不同的是作为输入函数的参数的回调函数要有error参数：callback(erro,result).

bindNodeCallback也不是一个真正意义上的操作符，因为它的输入和输出不是Observable。 输入是一个带有一些参数的函数func，最后一个参数是func调用完成后的回调函数。 回调函数应该遵循Node.js约定，其中回调的第一个参数是错误，而剩余的参数是回调结果。 bindNodeCallback的输出是一个函数，它使用与func相同的参数，但是除去最后一个（回调）。 当使用参数调用输出函数时，它将返回一个Observable，其中结果将被传递。

eg：

```js
import * as fs from 'fs';
var readFileAsObservable = Rx.Observable.bindNodeCallback(fs.readFile);
var result = readFileAsObservable('./roadNames.txt', 'utf8');
result.subscribe(x => console.log(x), e => console.error(e));
```