### throw

- 语法：
                
```ts
public static throw(error:any,scheduler:Scheduler):Observable
```

- 功能：
创建一个只发出error通知的Observable。

![](/assets/throw.png)


> throw操作符对于创建一个只发出error通知的Observable非常有用。它可以用于与其他Observable合并，如在mergeMap中。

eg1:

```js
var result = Rx.Observable.throw(new Error('oops!')).startWith(7);
result.subscribe(x => console.log(x), e => console.error(e));
```
**result:**
![](/assets/throw-result.png)

eg2:

```js
var interval = Rx.Observable.interval(1000);
var result = interval.mergeMap(
  function(x) {
    return x === 13 ? Rx.Observable.throw('Thirteens are bad') : Rx.Observable.of('a', 'b', 'c');
});
result.subscribe(
  function(x){ console.log(x)},
  function(e){console.error(e)}
);
```

eg2-result:
![](/assets/throw-result2.png)

