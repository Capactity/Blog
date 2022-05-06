# EventLoop设计原理

> `Event Loop`即事件循环。是指浏览器或`Node`的一种解决`Javascript`单线程运行时不会阻塞的一种机制，即我们经常使用异步的原理，具体分为宏任务和微任务。



**宏任务（MacroTask）:**  `setTimeout`、`setInterval`、`requestAnimationFrame`、**Ajax**、**fetch**、`script 标签代码`、`I/O操作`、`UI渲染`等。

**微任务（MicroTask）:**  `Promise.then`、`MutationObserver`、`Object.observe`。



## 浏览器中的Event Loop

> 在浏览器环境下，当某个宏任务执行完后，会查看是否有微任务队列，如果有，先执行微任务队列中的所有任务，如果没有，会读取宏任务队列中排在最前的任务，执行宏任务的过程中，遇到微任务，会依次加入微任务队列。当栈空后，再此读取微任务队列里的任务，依次类推。



<img src="../img/浏览器事件循环.png" alt="浏览器事件循环"  />





````typescript
Promise.resolve().then(()=>{
  console.log('Promise1')
  setTimeout(()=>{
    console.log('setTimeout2')
  },0)
})
setTimeout(()=>{
  console.log('setTimeout1')
  Promise.resolve().then(()=>{
    console.log('Promise2')
  })
},0)

// 输出结果：
Promise1
setTimeout1
Promise2
setTimeout2
````

- ​	先执行同步任务，执行完毕。查看是否有微任务，执行微任务队列的`Promise1`，同时生成一个宏任务`setTimeout2`。
- 查看宏任务队列，宏任务`setTimeout1`在`setTimeout2`之前，先执行`setTimeout1`，输出`setTimeout1`。
- 在执行宏任务`setTimeout1`是会生成微任务`Promise2`，放入微任务队列中，接着先去清空微任务队列中的所有任务，输出`Promise2`。
- 清空完微任务队列中的任务后，又去宏任务队列中取一个，执行并输出`setTimeout2`。



## Node中的Event Loop





## Node与浏览器的Event Loop差异

> 浏览器环境下，微任务的任务队列是每个宏任务执行完之后执行。

> 在node中，微任务会在时间循环的各个阶段之间执行，也就是一个阶段执行完毕，就会执行微任务队列中的任务。

