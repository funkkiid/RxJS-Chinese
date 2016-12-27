###create

语法:
```
public static create(subscribe: function(subscriber: Subscriber): TeardownLogic): Observable
```

功能:

创建一个新的Observable，当被订阅时，它将执行指定的函数。