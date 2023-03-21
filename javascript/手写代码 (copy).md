## 手写instanceof方法
instanceof运算符用于判断构造函数的prototype属性是否出现在对象的原型链中的任何位置
```js
function myInstanceOf(left, right) {
  let proto = Object.getPrototypeOf(left), // 获取对象的原型
      prototype = right.prototype; // 获取构造函数的prototype对象
  // 判断构造函数的prototype对象是否在对象的原型链上
  while(true) {
    if (!proto) return false;
    if (proto === prototype) return true;
    proto = Object.getPrototypeOf(prototype);
  }
}
```
## 手写new操作符
在调用 new的过程中会发生以上四件事情：
1.首先创建一个新的空对象
2.设置原型，将对象的原型设置为函数的prototype对象
3.修改this指向
4 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，则返回这个引用类型的对象。

```js
function objectFactory() {
  let newObject = null;
  let constructor = Array.prototype.shift.call(arguments);
  let result = null;
  // 判断参数是否是一个函数
  if (typeof constructor !== "function") {
    console.error("type error");
    return;
  }
  // 新建一个空对象，对象的原型为构造函数的 prototype 对象
  newObject = Object.create(constructor.prototype);
  // 将 this 指向新建对象，并执行函数
  result = constructor.apply(newObject, arguments);
  // 判断返回对象
  let flag = result && (typeof result === "object" || typeof result === "function");
  // 判断返回结果
  return flag ? result : newObject;
}
// 使用方法
objectFactory(构造函数, 初始化参数);
```
## 手写Promise.all
1) 核心思路
接收一个 Promise 实例的数组或具有 Iterator 接口的对象作为参数
这个方法返回一个新的 promise 对象，
遍历传入的参数，用Promise.resolve()将参数"包一层"，使其变成一个promise对象
参数所有回调成功才是成功，返回值数组与参数顺序一致
参数数组其中一个失败，则触发失败状态，第一个触发失败的 Promise 错误信息作为 Promise.all 的错误信息。
```js
function promiseAll(promises) {
  return new Promise(function(resolve, reject) {
    if(!Array.isArray(promises)){
        throw new TypeError(`argument must be a array`)
    }
    var resolvedCounter = 0;
    var promiseNum = promises.length;
    var resolvedResult = [];
    for (let i = 0; i < promiseNum; i++) {
      Promise.resolve(promises[i]).then(value=>{
        resolvedCounter++;
        resolvedResult[i] = value;
        if (resolvedCounter == promiseNum) {
            return resolve(resolvedResult)
          }
      },error=>{
        return reject(error)
      })
    }
  })
}
// test
let p1 = new Promise(function (resolve, reject) {
    setTimeout(function () {
        resolve(1)
    }, 1000)
})
let p2 = new Promise(function (resolve, reject) {
    setTimeout(function () {
        resolve(2)
    }, 2000)
})
promiseAll([p1, p2]).then(res => {
    console.log(res) // [1, 2]
})
```
## call 函数的实现步骤
判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
判断传入上下文对象是否存在，如果不存在，则设置为 window 。
处理传入的参数，截取第一个参数后的所有参数。
将函数作为上下文对象的一个属性。
使用上下文对象来调用这个方法，并保存返回结果。
删除刚才新增的属性。
返回结果。
```js
// call函数实现
Function.prototype.myCall = function(context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
  }
  // 获取参数
  let args = [...arguments].slice(1),
      result = null;
  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;
  // 将调用函数设为对象的方法
  context.fn = this;
  // 调用函数
  result = context.fn(...args);
  // 将属性删除
  delete context.fn;
  return result;
};
```

## bind 函数的实现步骤
判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
保存当前函数的引用，获取其余传入参数值。
创建一个函数返回
函数内部使用 apply 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 this 给 apply 调用，其余情况都传入指定的上下文对象。


## 手写防抖函数
函数防抖是指在事件被触发 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时。这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。
```js
// 函数防抖的实现
function debounce(fn, wait) {
  let timer = null;

  return function() {
    let context = this,
        args = arguments;

    // 如果此时存在定时器的话，则取消之前的定时器重新记时
    if (timer) {
      clearTimeout(timer);
      timer = null;
    }

    // 设置定时器，使事件间隔指定事件后执行
    timer = setTimeout(() => {
      fn.apply(context, args);
    }, wait);
  };
}
```
## 手写节流函数
函数节流是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。节流可以使用在 scroll 函数的事件监听上，通过事件节流来降低事件调用的频率。
```js
// 函数节流的实现;
function throttle(fn, delay) {
  let curTime = Date.now();

  return function() {
    let context = this,
        args = arguments,
        nowTime = Date.now();

    // 如果两次时间间隔超过了指定时间，则执行函数。
    if (nowTime - curTime >= delay) {
      curTime = Date.now();
      return fn.apply(context, args);
    }
  };
}
```
## 实现数组的乱序输出
取出数组的第一个元素，随机产生一个索引值，将该第一个元素和这个索引对应的元素进行交换。
第二次取出数据数组第二个元素，随机产生一个除了索引为1的之外的索引值，并将第二个元素与该索引值对应的元素进行交换
按照上面的规律执行，直到遍历完成
```js
var arr = [1,2,3,4,5,6,7,8,9,10];
for (var i = 0; i < arr.length; i++) {
  const randomIndex = Math.round(Math.random() * (arr.length - 1 - i)) + i;
  [arr[i], arr[randomIndex]] = [arr[randomIndex], arr[i]];
}
console.log(arr)
```
## 实现数组的扁平化
（1）递归实现
普通的递归思路很容易理解，就是通过循环递归的方式，一项一项地去遍历，如果每一项还是一个数组，那么就继续往下遍历，利用递归程序的方法，来实现数组的每一项的连接
（2）split 和 toString 
可以通过 split 和 toString 两个方法来共同实现数组扁平化，由于数组会默认带一个 toString 的方法，所以可以把数组直接转换成逗号分隔的字符串，然后再用 split 方法把字符串重新转换为数组
（3）正则和 JSON 方法
在第2种方法中已经使用 toString 方法，其中仍然采用了将 JSON.stringify 的方法先转换为字符串，然后通过正则表达式过滤掉字符串中的数组的方括号，最后再利用 JSON.parse 把它转换成数组



## React18 有哪些更新

#### 1 setState 自动批处理

React17 中，只有 react 事件会进行批处理，原生 j s 事件，promise、setTimeout、setInterval 等则不会。

**React18 中，将所有事件都进行批处理，即多次 setState 会被合并为 1 次执行，提高了性能**。在数据层，将多次状态更新合并成一次处理（在视图层，将多次渲染合并成一次渲染）。

#### 2 引入新 root API,支持 new concurrent renderer（并发模式的渲染）

#### 3 strict mode 更新

**当使用严格模式时，React 对每个组件返回两次渲染**，在 React17 中去掉了一次渲染的控制台日志。React18 取消了这个限制，第二次渲染会以浅灰色出现在控制台日志。

#### 4 React18 在 render 中删除了回调函数，如果需要使用回调，可以通过 useEffect 实现。

#### 5 Concurrent Mode

它可以帮助应用保持响应。通过渲染可终端来修复阻塞渲染机制。在并发模式中，react 可以同时更新多个状态，**使同步不可中断更新变成了异步可中断更新**。

#### 6 Suspence 不再需要 fallback 捕获

**Suspence 的作用是划分页面中需要并发渲染的部分，和 lazy 一起使用服务于优化打包，可以显著减少主包体积，提升用户体验。**

React17 中，如果 Suspence 没有提供 fallback 属性，React 会跳过继续向上搜索下一个边界，这可能导致难以调试。比如 debug 在没有 fallback 的 Suspence 组件中抛出一个边界问题进行测试，不会触发警告报错。

React18 将当前组件的 Suspence 的值默认设置为 null 或 undefined。

## React 设计思想

- 组件化 - 组件符合开闭原则，渲染工作流封闭，组件通信通过 props 进行数据交互开放。
- 数据驱动视图 - UI = f(data) 通过修改数据（state 或 prop），数据驱动视图更新。
- 虚拟 DOM - 是对真实 DOM 的映射，通过新旧虚拟 DOM 对比，得到需要更新部分，实现数据增量更新。

## React 生命周期

组件生命周期分为**挂载**、**更新**、**卸载**阶段

**挂载：**

- **constructor** 进行 state 和 props 的初始化
- render
- **componentDidMount** 第一次渲染后调用，可以访问 DOM 进行异步请求和定时器、消息订阅。

**更新：**

- 组件的 state 或 props 变化时触发更新
- **shouldComponentUpdate**返回布尔值，默认为 true。设置 true / false 确认是否更新组件，提高性能。
- render
- **componentDidUpdate** 在组件更新后调用

**卸载：**

- **componentWillUnMount** 组件从 DOM 中被移除时调用

**错误捕获：**

- **componentDidCatch**

## state 和 props 触发更新的生命周期分别有什么区别？

区别在于props增加了componentWillReceiveProps调用，用来比较当次porps和下次的是否相同。

## React 事件机制

> 通过在id=root处监听所有事件，当事件冒泡到id=root时，react将事件内容封装合成交由真正的处理函数运行，通过事件池复用而不是创建事件对象，解决了内存分配问题。
>
> react17之后，事件都委托在id=root的dom元素上，也不再使用事件池了。

React 基于浏览器实现的事件机制，包含**事件触发**、**事件冒泡**、**事件捕获**、**事件合成**和**事件派发**。

#### react 事件机制和原生 DOM 事件流有什么区别？

虽然合成事件不是原生 DOM 事件，但它包含了原生 DOM 事件的引用，可以通过 e.nativeEvent 访问。

#### **dispatchEvent 是怎么实现事件分发的**？

事件触发的本质是对dispatchEvent函数的调用，给事件对象绑定自定义事件。



## React性能优化手段

- 组件设置是否更新 - **shouldComponentUpdate**
- useMemo
- getDerviedStateFromProps
- 使用Fragment - 允许你将子列表分组，而无需向 DOM 添加额外节点
- v-for使用正确的key
- 拆分尽可能小的可复用组件，ErrorBoundary
- 使用React.lazy和React.Suspense延迟加载不需要立马使用的组件

## React组件间通讯方式

#### **父 > 子通信：**

- props传递

#### **子 > 父通信：**

- 回调函数，父组件向子组件传递一个函数，通过函数回调，拿到子组件传过来的值
- 事件冒泡，点击子组件的button按钮，事件会冒泡到父组件上
- Ref，createRef创建ref。将其赋值给一个变量，通过ref挂载在dom节点或组件上，该ref的current属性将拿到dom节点或组件的实例

#### 兄弟组件通信：

- 实际上就是通过父组件中转数据的，子组件a传递给父组件，父组件再传递给子组件b

#### **父组件向后代组件通信**

- Context ：相当于大容器，通信内容放在容器里，不管嵌套多深，可以随意取用。

## React hooks

**优点：**

- 告别难以理解的class组件
- 解决业务逻辑难以拆分的问题
- 使状态逻辑复用变得简单可行
- 函数组件的设计理念更符合React规范

**局限性：**

- hooks不能完整的为函数组件提供类组件的能力
- 函数组件要求行更高
- hooks在使用层面有严格的规则约束



## React 高阶组件HOC、Render props、hooks 有什么区别

- HOC：一种组件设计模式，纯函数。优缺点：逻辑复用且独立 / 重名组件会被覆盖
- props：组件间元素函数或值传递。优缺点：数据共享 / 无法在return语句外访问
- hooks：解决了hoc的prop覆盖及回调地狱问题。优缺点：解决代码复用问题 / 只能在组件顶层使用



## React useEffect 怎么支持 async ....await...

在回调中使用async会报错，useEffect应该返回销毁函数，用async返回值会变成promise，导致报错。

**需要支持的用法：**在useEffect中创建一个异步函数，并且马上执行。



## React 类组件与函数式组件区别

- 类组件有生命周期，函数组件没有
- 类组件需要继承 Class，函数组件不需要
- 类组件可以获取实例化的 this，并且基于 this 做各种操作，函数组件不行
- 类组件内部可以定义并维护 state， 函数组件都称为无状态了，那肯定不行。
- 性能优化上：类组件依靠shouldComponentUpdate阻断，函数组件依靠memo缓存渲染结果。

## 受控组件和非受控组件

**受控组件**：比如表单input，当表单状态变化时，会触发onchange事件，更新组件state。组件的状态与value对应，消除了局部状态。

优缺点：值由react组件管理，当组件过多时，需要单独写事件处理函数，代码臃肿。

**非受控组件**：如果表单中没有value prop时，用ref来获取表单值，就不需要单独写数据维护函数。

总结：页面中所有输入如果是现用现取的称为非受控组件，而通过setState维护的称为受控组件。

## setState是同步还是异步

**setState是一个异步方法，但是在setTimeout / setInterval等定时器里会逃脱React的掌控，变成同步方法。**

实现机制类似于事件循环，每个setState都会被加入到任务队列。

多次对同一个state使用setState指挥返回最后一次的结果，先放在队列里，等时机成熟后执行批量更新。

React18使用了createRoot api后，所有setState都是异步批量执行。

## state和props有什么区别？

- props：外部传入，可读性、不变性，仅外部传入新值重新渲染。
- state：构造器中初始化，私有属性，不可外部访问或修改。修改会导致重新渲染。

## mobx和redux有什么区别？

**redux**： 工作流程，createStore  -->  action定义行为   ---> dispatch触发事件      ---> reducer 处理state, 返回新的state

**mobx**：工作流程，createStore  -->  action定义行为 --- > 监听事件   --->   触发订阅 ---> 修改state值

redux 是在 reducer 函数里组织状态，mobx 则是在 class 里组织状态。

## useState为什么用数组而不是对象？

主要是为降低使用复杂度，返回数组可以直接解构赋值，对象的话多次使用需要定义别名。

## hooks实现原理

hooks基于函数式组件，将数据和渲染绑定，解决了纯函数组件无法实现全功能组件的问题，以钩子的形式提供。

相比class组件成本高，组件复杂和难以拆分复用的问题。

解决的问题：

1. 组件之前复用状态逻辑
2. 拆分更小粒度函数，降低复杂组件难度
3. 拥抱函数，比class组件更易于理解

局限性：

- 不能在循环、条件或嵌套函数中调用hook。 --- hook基于数组实现，循环等条件调用可能导致数组取值错位，执行错误的hook。
- 只能在react的函数组件中调用hook。 --- hook基于函数组件设计。



## React的事件代理机制？

事件委托：当节点数量较多时，如果给每个节点都绑定，内存消耗大，绑定在其父节点统一处理，减少绑定数量。

react17之后，事件都委托在id=root的dom元素上，也不再使用事件池，在代理阶段，对dom的事件委托分别在捕获阶段和冒泡阶段。



## React性能优化手段

- shouldComponentUpdate判断是否渲染，子组件由父组件决定是否重绘。
- props设置数组或对象，因为每次react组件都会创建新组件，可以优先声明值，再读取赋值。
- 将函数绑定移到构造函数里，避免每次渲染都绑定。
- 设置唯一key值。



## React懒加载配置

通过Suspense和lazy实现

1. 在App组件中，导入Suspense组件，并用Suspense包裹组件。
2. 提供fallback属性，指定loading占位内容。



## Fiber架构

#### 什么是Fiber，Fiber解决了什么问题？

**在React16以前，更新是通过树的深度优先遍历完成的**，遍历无法中断，当树的层级过深会产生栈的层级过深，页面渲染速度变慢。

故引入了Fiber，**React Fiber就是虚拟DOM，他是一个链表，返回return、children、siblings**，分别代表父Fiber，子Fiber和兄弟Fiber。

**Fiber是纤程，比线程更精细，表示对渲染线程实现更精细的控制，具备可中断、可恢复的特性。**

#### 应用目的

实现增量渲染，即是把一个渲染任务分解为多个渲染任务，具备可中断、可恢复的特性，按照优先级处理任务，达到更顺滑的用户体验。

#### **Fiber的可中断、可恢复怎么实现的**

Fiber是协程，比线程更小的单元，可以人为中断和恢复。因此我们通过Fiber把浏览器渲染分段执行，每执行一段时间就让出主线程控制权，执行优先级更高的任务。

Fiber是链表结构，有三个指针，分别记录当前节点的下一个兄弟节点、子节点、父节点。当遍历中断时，只需要保留当前节点的索引，就能根据索引找到对应的节点，恢复遍历。

#### **Fiber更新机制**

**初始化：**

1. 创建FiberRoot（React根元素）和rootFiber（通过ReactDOM.render 或 ReactDOM.createRoot创建出来）
2. 进入beginWork

**workInProgress**: 正在内存中构建的Fiber树，第一次更新时，所有的更新都发生在workInProgress树中，第一次更新后，workInProgress树的状态是最新状态，它会替换current树。

**current**:正在视图层渲染的树叫current Fiber树

```
currentFiber.alternate = workInProgressFiber
workInProgressFiber.alternate = currentFiber
```

3. 深度调和子节点，渲染视图

在新建的alternate树上，完成整个子节点的遍历，包括Fiber的创建，最后会以workInProgress树为最新的渲染树，FiberRoot的current指针指向workInProgress使其变成current Fiber，完成初始化流程。

**更新：**

重新创建虚拟DOM树（workInProgress树），复用当前视图层上的alternate属性，作为新的workInProgress。



#### React渲染流程

如图，React用JSX描述页面，经过babel编译为render function，执行后产生VDOM，VDOM不是直接渲染的，会先转换为Fiber，再进行渲染。vdom转换为Fiber的过程叫reconcile，转换过程会创建DOM，全部转换完成后会一次性commit到DOM,这个过程不是一次性的，是可打断的。

vdom（React Element对象）中只记录子节点，不记录兄弟节点，因此渲染不可打断

Fiber（FiberNode对象）是一个链表，它记录了父节点、兄弟节点、子节点，因此是可以打断的



## 判断两条线段是否相交

判断两条连线的向量乘是否为0，为0则表示平行，否则相交

## 如何快速选中三角形、多边形

<img src="/Users/cw/Desktop/Blog/img/三角形.png" alt="三角形"  />



#### **1 面积法**

如果点p在三角形ABC内部，则三个小三角形面积PAB、PBC、PAC 之和等于ABC的面积，反之则不等。

#### **2 向量法**

可以使用向量的叉乘，假设三角形的三个点按照顺时针顺序为A、B、C。对于某一点P，求出三个向量PA、PB、PC:

t1 = PA^PB,

t2 = PB^PC,

t3 = PC^PA,

如果t1，t2，t3同号（同正或同负）则方向相同，说明点P在三角形每条边的同侧即内部，那么P在三角形内部，否则在外部。

### 判断点是否在多边形内

#### 1 面积法

判断目标点与多边形组成的三角形面积和是否等于该多边形。

**不规则多边形计算面积公式**：求每条线段下面与x轴之间的阴影面积，求两个高度的平均值，再乘以宽度。最后把所有面积加起来，注意，前进时的面积要加上，后退时的面积要减去。

<img src="/Users/cw/Desktop/Blog/img/多边形1.png" alt="多边形1"  />

<img src="/Users/cw/Desktop/Blog/img/多边形2.png" alt="多边形2"  />



#### 2 引射线法

从目标点出发引一条射线，看这条射线和多边形所有边的交点数目，如果为奇数则在多边形，反之则在物体外。

## WebGLRenderer的渲染过程

简单理解就是，把我们绘制的场景图，通过相机视椎体的范围来截取，转化为一张图片。然后去绑定页面上`canvas元素`把这张图片绘制到元素上。动画效果就是不断的生成新的图片替换原来的图片来实现的。渲染器上有很多属性和方法，都是配合其他内容一起使用的，比如阴影就需要灯光和几何体来配合才能展示。

**WebGLRenderer** 把渲染流程分为三个部分。下面的源码已经精简过，并带上了一些注释, 可以参考流程图试着去走一遍流程。

- 预处理：对该次需要渲染物体数据进行初始化分类，根据不透明、透明、透射特性分组、预添加物体。
- 阴影、物体渲染：根据数据计算位置数量，调用webGL原生api渲染
- 后处理：清理过程变量，清理数组及状态。

![render-stages.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f390f63339c94b29b75eae890ae08623~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)



## 三维物体如何判断选中

three.js的物体选中，主要是通过光线投射Raycaster进而运算得到

1. 初始化 raycaster = new THREE.Raycaster
2. 监听鼠标位置，获取鼠标位置，通过将三维坐标转屏幕坐标 new THREE.Vector3(x,y,z)
3. 更新射线，计算相机照射到鼠标的光线，找到和射线相交的对象，返回数组。

