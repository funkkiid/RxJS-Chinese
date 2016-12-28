###fromPromise

语法:


```
public static fromPromise(promise: Promise<T>, scheduler: Scheduler): Observable<T>
```

功能: 转化一个Promise为一个Obseervable

将ES2015 Promise或Promises / A +规范符合转换为Observable。 如果Promise为成功状态，则Observable会将成功的值作为next发出，然后complate。 如果Promise被失败，则输出Observable发出相应的错误。




eg:




```
//convert the Promise returned by Fetch to an Observable

var result = Rx.Observable.fromPromise(fetch('http://myserver.com/'));
result.subscribe(x => console.log(x), e => console.error(e));
```

