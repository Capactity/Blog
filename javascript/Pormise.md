# Promise实现

> Promise是异步编程的一种解决方案，ES6将其写进语言标准，原生提供了Promise对象。



## Promise/A+ 规范

> ES6主要用的是Promise/A+规范，该规范内容较多，如下：

1. 每个Promise实例只能有三个状态，（等待）`pending`、（执行）`fulfilled`、（拒绝）`reject`，状态之间的转化只能是`pending => fulfilled`， `pending =>reject`，状态变化不可逆。
2. Promise有一个`then`方法，该方法可以被调用多次，并且返回一个Promise对象。
3. 支持链式调用。
4. 内部保存有一个`value`值，用来保存上次执行的结果值，如果报错，则保存的是异常结果信息。

