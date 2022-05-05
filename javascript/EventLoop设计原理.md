# EventLoop设计原理

> `Event Loop`即事件循环。是指浏览器或`Node`的一种解决`Javascript`单线程运行时不会阻塞的一种机制，即我们经常使用异步的原理，具体分为宏任务和微任务。



**宏任务（MacroTask）:**  `setTimeout`、`setInterval`、`requestAnimationFrame`、**Ajax**、**fetch**、`script 标签代码`。

**微任务（MicroTask）:**  `Promise.then`、`MutationObserver`、`Object.observe`。

