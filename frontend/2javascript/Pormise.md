# Promise实现

> Promise是异步编程的一种解决方案，ES6将其写进语言标准，原生提供了Promise对象。



## **setTimeout、Promise、Async/Await 的区别**

Promise本身是同步的立即执行函数，当在executor中执行resolve或者reject的时候, 此时是异步操作， 会先执行then/catch等。

async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。



## async/await对比Promise的优势

●代码读起来更加同步，Promise虽然摆脱了回调地狱，但是then的链式调⽤也会带来额外的阅读负担 
●Promise传递中间值⾮常麻烦，⽽async/await⼏乎是同步的写法，⾮常优雅 
●错误处理友好，async/await可以⽤成熟的try/catch，Promise的错误捕获⾮常冗余 
●调试友好，Promise的调试很差，由于没有代码块，你不能在⼀个返回表达式的箭头函数中设置断点，如果你在⼀个.then代码块中使⽤调试器的步进(step-over)功能，调试器并不会进⼊后续的.then代码块，因为调试器只能跟踪同步代码的每⼀步。 

## Promise.all实现

核心思路
接收一个 Promise 实例的数组或具有 Iterator 接口的对象作为参数
这个方法返回一个新的 promise 对象，
遍历传入的参数，用Promise.resolve()将参数"包一层"，使其变成一个promise对象
参数所有回调成功才是成功，返回值数组与参数顺序一致
参数数组其中一个失败，则触发失败状态，第一个触发失败的 Promise 错误信息作为 Promise.all 的错误信息。



## Promise.all() 和 Promise.race() 的区别

Promise.all() 方法接受一个可迭代对象（比如数组），并返回一个 Promise 实例。该实例在参数中所有的 Promise 都成功 resolve 后才会 resolve，并把每个 Promise 的结果按照传入的顺序组成一个数组作为返回值；如果有任何一个 Promise 被 reject，则 Promise.all() 返回的 Promise 实例会立即 reject 并返回第一个被 reject 的 Promise 的结果。

Promise.race() 方法同样也接受一个可迭代对象，并返回一个 Promise 实例。但是它和 Promise.all() 不同的是，只要参数中的任何一个 Promise 实例 resolve 或 reject，它就会立即 resolve 或 reject，而不管其他的 Promise 是否已经 resolve 或 reject 了。返回值是第一个完成的 Promise 的结果。

因此，Promise.all() 和 Promise.race() 主要的区别在于它们返回结果的方式不同。Promise.all() 返回一个数组，包含所有 Promise 的 resolve 结果，而 Promise.race() 返回第一个完成的 Promise 的结果。



Promise.all() 通常用于在多个异步操作都完成后才执行某些操作的情况，比如需要等待多个网络请求返回数据后再更新 UI。

而 Promise.race() 则适合在多个异步操作中只需取得一个完成的操作的结果时使用。比如，在从多个可用服务器上获取数据时，我们希望尽快获取数据，而不是等到所有服务器都返回数据后再选择。



## Promise/A+ 规范

> ES6主要用的是Promise/A+规范，该规范内容较多，如下：

1. 每个Promise实例只能有三个状态，（等待）`pending`、（执行）`fulfilled`、（拒绝）`reject`，状态之间的转化只能是`pending => fulfilled`， `pending =>reject`，状态变化不可逆。
2. Promise有一个`then`方法，该方法可以被调用多次，并且返回一个Promise对象。
3. 支持链式调用。
4. 内部保存有一个`value`值，用来保存上次执行的结果值，如果报错，则保存的是异常结果信息。





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





## 实现

由于Promise为状态机，我们需先定义状态

````typescript
const PENDING = 0; // 进行中
const FULFILLED = 1; // 成功
const REJECTED = 2; // 失败
````



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
function Promise(fn){
  ...
  // 对不同状态进行不同处理
  function handle(handler) {
    if(state === PENDING) {
      handlers.push(handler);
    } else {
      // 成功状态
      if (state === FULFILLED && typeof handler.onFulfilled === 'function') {
        handler.onFulfilled(value);
      }
      // 失败状态
      if (state === REJECTED && typeof handler.onRejected === 'function') {
        handler.onRejected(value);
      }
    }
  }
  
  // 当Promise被resolved或rejected时，保证handlers将被通知
  this.done = function (onFulfilled, onRejected) {
    // 保证异步执行
    setTimeout(function() {
      handle({onFulfilled: onFulfilled, onRejected.onRejected});
    }, 0);
  }
}
````



##### then方法

````typescript
function Promise(fn){
  ...
  this.then = function(onFulfilled, onRejected) {
    const _this = this;
    return new Promise(function (resolve, reject){
      _this.done(function(result){
        // onFulfilled 需要设置返回
        if (typeof onFulfilled === 'function') {
          try {
            return resolve(onFulfilled(result));
          } catch (ex) {
            return reject(ex);
          }
        } else {
          return resolve(result);
        }
      },function (error) {
        if (typeof onRejected === 'function') {
          try {
            return resolve(onRejected(result));
          } catch (ex) {
            return reject(ex);
          }
        } else {
          return reject(error);
        }
      })
    })
  };
  
  // 对应的catch方法， 直接调用then处理异常
  this.catch = function(errorHandle) {
    return this.then(null, errorHandle)
  }
  
}
````



以上为promise基本实现原理。

