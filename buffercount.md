###bufferCount

语法：
```
public bufferCount(bufferSize: Number,startBufferEvery):Observable<T[]>
```

功能：缓存原始observable发射的值，直到达到bufferSize给定的上限

![](/assets/bufferCount.png)



eg:
```
//每点击2次，发射一次由之前点击事件组成的数组
var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferCount(2);
buffered.subscribe(x => console.log(x));
```

eg-result:

![](/assets/bufferCount-result.png)



eg-2(第二个参数的作用)：
```
//首次点击3次，发射一次由之前三次点击事件组成的数组。之后，每点击两次发射一次重置产生新数组，新数组的长度为3，第一个元素为上次发射数组的最后一个元素，后两个元素为本次2次点击事件。具体看结果截图，注意观察点击事件发生的**位置**。

var clicks = Rx.Observable.fromEvent(document, 'click');
var buffered = clicks.bufferCount(2, 1);
buffered.subscribe(x => console.log(x));
```
eg-2-result:
发射的为三个事件元素组成的数组(即第一个参数设定的值)
![](/assets/bufferCount-result2-1.png)
注意第个数组第三次点击事件发生的**位置**
![](/assets/bufferCount-result2-2.png)
继续观察第二个数组，注意第一个事件元素发生的位置
![](/assets/bufferCount-result2-3.png)

总结一个：第一个参数为返回数组的长度，第二个为再发射几个值重置并产生新的数组。

**此外**，第二个参数的值可以**大于**第一个参数，比如，arg1=3，arg2=4。以上例为基础，这种情况会发生如下的事情:
1、初次点击三次，发射一个数组

2、再点击四次，发射第二个数组

3、...重复步骤2...

这种情况的出现，有一件事就非常值得关注，在步骤2之后每一次发射的数组，我们都点击了四次，但是数组只会包含三个点击事件，是哪三个呢？答案是四次点击的后三个，也就是四次中的第一次会被忽略。

下面就用事实来验证在：
首先我会在中间位置点击三次，产生一个数组，然后点击四次产生第二个数组、这四次的点击位置一次为:右上->左上->左下->右下

然后我们可以查看所包含的三次点击事件所发生的的位置

![](/assets/buffer-result2-9.png)

第一个很明显是左上

![](/assets/bufferCount-result2-8.png)

第二个很明显是左下

![](/assets/bufferCount-result2-7.png)

第三个很明显是右下

所以我们失去了四次点击的第一次点击。





