###bindNodeCallback

将一个NodeJS风格的回调函数API转化为一个能返回可观察对象的函数；

>基本等同于bindCallback,不同的是作为输入函数的参数的回调函数要有error参数：callback(erro,result).