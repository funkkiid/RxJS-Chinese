###throw

语法：
                
```
public static throw(error:any,scheduler:Scheduler):Observable
```

功能：创建一个只发出error通知的Observable。

![](/assets/throw.png)


>throw操作符对于创建一个只发出error通知的Observable非常有用。它可以用于与其他Observable合并，如在mergeMap中。

```
var result = Rx.Observable.throw(new Error('oops!')).startWith(7);
result.subscribe(x => console.log(x), e => console.error(e));
```
**result:**
![](/assets/throw-result.png)