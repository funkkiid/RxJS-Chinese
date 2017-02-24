### combineLatest

- 语法：

```ts
public static combineLatest(observable1: Observable, observable2: Observable, project: function, scheduler: Scheduler): Observable
```

- 功能:

组合多个Observable产生一个新的Observable，其发射的值根据其每个输入Observable的最新值计算。

> 无论何时作为输入的Observable发出一个值，它取所有输入的最新值作为它的发射值。
![](/assets/a1.png)

eg:
```js
// Dynamically calculate the Body-Mass Index from an Observable of weight and one for height
var weight = Rx.Observable.of(70, 72, 76, 79, 75);
var height = Rx.Observable.of(1.76, 1.77, 1.78);
var bmi = Rx.Observable.combineLatest(weight, height, (w, h) => w / (h * h));
bmi.subscribe(x => console.log('BMI is ' + x));
```
