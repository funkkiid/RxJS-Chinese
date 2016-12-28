###fromEventPattern

语法：


```
Rx.Observable.fromEventPattern(addHandler, [removeHandler], [selector])
```

功能:
通过使用addHandler和removeHandler函数添加和删除处理程序。 当输出Observable被订阅时，addHandler被调用，并且当订阅被取消订阅时调用removeHandler。
![](/assets/fromEventPattern.png)



```
function addClickHandler(handler) {
  document.addEventListener('click', handler);
}

function removeClickHandler(handler) {
  document.removeEventListener('click', handler);
}

var clicks = Rx.Observable.fromEventPattern(
  addClickHandler,
  removeClickHandler
);
clicks.subscribe(x => console.log(x));
```

