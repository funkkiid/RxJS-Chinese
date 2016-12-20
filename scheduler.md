##Scheduler调度者
什么是调度者？调度者控制着何时启动一个订阅和何时通知被发送。它有三个组件构成。
* **一个调度者是一个数据结构**。它知道如何根据优先级或其他标准存储和排列任务。
* **一个调度者是一个执行上下文**。它表示何处何时任务被执行(例如: immediately(立即), or in another callback mechanism(回调机制) such as setTimeout or process.nextTick, or the animation frame)
* **一个调度者具有虚拟的时钟**。它通过调度器上的getter方法now（）提供了“时间”的概念。 在特定调度程序上调度的任务将仅仅遵守由该时钟表示的时间。