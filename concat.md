###concat

语法:
```
public static concat(input1: Observable, input2: Observable, scheduler: Scheduler): Observable
```
功能：产生一个Observable，它取作为参数的Observable发射的每个值，并顺序(基于作为输入的可观察对象的顺序)发出所有值。

>连接多个可观察对象，并顺序发出他们的值，一个可观察对象跟在另一个的后面