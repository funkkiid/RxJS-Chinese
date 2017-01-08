###catch

语法:
```
publci catch(selector:function):Observable
```
参数为一个函数:
它接受作为参数err，这是错误，并捕获。
需要注意的是，如果使用了该操作符，observer的error方法将不会被执行，因为错误已经被不catch操作符捕获。
此外，不可以使用try...catch，因为此代码块是同步执行的。