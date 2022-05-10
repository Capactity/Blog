# Promise实现

> Promise是异步编程的一种解决方案，ES6将其写进语言标准，原生提供了Promise对象。



## Promise/A+ 规范

> ES6主要用的是Promise/A+规范，该规范内容较多，如下：

1. 每个Promise实例只能有三个状态，（等待）`pending`、（执行）`fulfilled`、（拒绝）`reject`，状态之间的转化只能是`pending => fulfilled`， `pending =>reject`，状态变化不可逆。
2. Promise有一个`then`方法，该方法可以被调用多次，并且返回一个Promise对象。
3. 支持链式调用。
4. 内部保存有一个`value`值，用来保存上次执行的结果值，如果报错，则保存的是异常结果信息。







## 实现

由于Promise为状态机，我们需先定义状态

````typescript
const PENDING = 0; // 进行中
const FULFILLED = 1; // 成功
const REJECTED = 2; // 失败
````



#### 基础使用

````typescript
const p1 = new Promise((resolve, reject) => {
  console.log(1);
  resolve(3);
  reject(4);
})

console.log(2);

const p2 = p1.then(data => {
  console.log(data)
  throw new Error(5)
})

const p3 = p2.then(data => {
  console.log('success', data)
}, err => {
  console.log('faild', err)
})

//输出顺序
1
2
3
faild Error: 5

````

上述代码不会输出4，是由于：

Promise的状态切换，pending => fulfilled 或 pending => rejected，状态一旦改变，不会再变。







#### 基本代码

````typescript
function Promise(fn) {
  let state = PENDING; // 存储初始状态
  let value = null; // 存储成功或失败的结果值
  let handlers = []; // 存储成功或失败的处理程序，通过调用 `.then` 或者 `.done` 方法
  
  // 成功状态
  function fulfill(result) {
    state = FULFILLED;
    value = result;
    handlers.forEach(handle); // 处理函数
    handlers = null;
  }
  
   // 失败状态
  function reject(error) {
    state = REJECTED;
    value = error;
    handlers.forEach(handle); // 处理函数
    handlers = null;
  }
}
````



#### 实现resolve方法

resolve方法可以接受两种参数，一种为普通的值/对象，另一种为一个Promise对象，如果是普通的值/对象，则直接把结果传递到下一个对象

如果是一个Promise对象，则必须先等待这个子任务序列完成。

````typescript
function Promise(fn) {
  ...
  function resolve(result) {
    try {
      const then = getThen(resule);
      // 为Promise对象时
      if (then) {
        doResolve(then.bind(result), resolve, reject)
        return;
      }
    } catch (e) {
      reject(e);
    }
  }
  ...
}
````



resolve需要两个辅助方法getThen、doResolve。

````typescript
// getThen判断是否为Promise对象，是则返回then方法等待执行完成
function getThen(value) {
  const t = typeof value;
  if (value && (t === 'object' || t === 'function')) {
    const then = value.then;
    if (then === 'fucntion') {
      return then;
    }
  }
  return null;
}

// 异常参数检查函数，确保onFulfilled和onRejected两个函数中只执行一个且只执行一次，但未做异步处理。
function doResolve() {
  let done = false;
  try {
    fn(
      function(value) {
        if (done) return;
        done = true;
        onFulfilled(value);
      },
      function(reason) {
        if(done) return;
        done = true;
        onRejected(reason);
      }
    );
  } catch (ex) {
    if(done) reutrn;
    done = true;
    onRejected(ex);
  }
}

// 解析Promise
function Promise(fn) {
  ...
  doResolve(fn, resolve, reject);
}
````



#### 实现then方法

首先实现一个执行方法done,该方法用来处理执行then方法的回调函数，promise.done(onFulfilled, onRejected)方法需要注意：

- onFulfilled和onRejected两者只能有一个被执行，且执行次数为一次。
- 该方法仅能被调用一次，一旦调用该方法，则promise链式调用结束。
- 无论promise是否被解析，该方法都可以被调用。

````typescript

````



