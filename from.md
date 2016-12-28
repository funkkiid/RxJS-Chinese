###from
语法:


```
public static from(ish: ObservableInput<T>, scheduler: Scheduler): Observable<T>
```

功能：将一个数组、类数组(字符串也可以)，Promise、可迭代对象，类可观察对象、转化为一个Observable

>可将几乎所有的东西转化一个可观察对象

![](/assets/from.png)

eg:


```
//数组=>Observable
var array = [10, 20, 30];
var result = Rx.Observable.from(array);
result.subscribe(x => console.log(x));
```



```
//无限迭代对象=>Observable
function* generateDoubles(seed) {
  var i = seed;
  while (true) {
    yield i;
    i = 2 * i; // double it
  }
}

var iterator = generateDoubles(3);
var result = Rx.Observable.from(iterator).take(10);
result.subscribe(x => console.log(x));
```

