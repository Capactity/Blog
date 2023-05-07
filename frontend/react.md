## React18 有哪些更新

#### 1 setState 自动批处理

React17 中，只有 react 事件会进行批处理，原生 j s 事件，promise、setTimeout、setInterval 等则不会。

**React18 中，将所有事件都进行批处理，即多次 setState 会被合并为 1 次执行，提高了性能**。在数据层，将多次状态更新合并成一次处理（在视图层，将多次渲染合并成一次渲染）。

#### 2 引入新 root API,支持 new concurrent renderer（并发模式的渲染）

```javascript
//React 17
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

const root = document.getElementById("root");
ReactDOM.render(root, <App />);

// 卸载组件
ReactDOM.unmountComponentAtNode(root);

// React 18
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
const root = document.getElementById("root");
ReactDOM.createRoot(root).render(<App />);

// 卸载组件
root.unmount();
```

#### 3 strict mode 更新

**当使用严格模式时，React 对每个组件返回两次渲染**，在 React17 中去掉了一次渲染的控制台日志。React18 取消了这个限制，第二次渲染会以浅灰色出现在控制台日志。

#### 4 React18 在 render 中删除了回调函数，如果需要使用回调，可以通过 useEffect 实现。

```javascript
// React 17
const root = document.getElementById('root')!;
ReactDOM.render(<App />, root, () => {
  console.log('finish render');
});

// React 18
const AppWithCallback: React.FC = () => {
  useEffect(() => {
    console.log('finish render (v18)');
  }, []);
  return <App />;
};
const root = document.getElementById('root')!;
ReactDOM.createRoot(root).render(<AppWithCallback />);
```

#### 5 Concurrent Mode

并发模式不是一个功能，而是一个底层设计。

它可以帮助应用保持响应。通过渲染可终端来修复阻塞渲染机制。在并发模式中，react 可以同时更新多个状态，**使同步不可中断更新变成了异步可中断更新**。

#### 6 Suspence 不再需要 fallback 捕获

**Suspence 的作用是划分页面中需要并发渲染的部分，和 lazy 一起使用服务于优化打包，可以显著减少主包体积，提升用户体验。**

React17 中，如果 Suspence 没有提供 fallback 属性，React 会跳过继续向上搜索下一个边界，这可能导致难以调试。比如 debug 在没有 fallback 的 Suspence 组件中抛出一个边界问题进行测试，不会触发警告报错。

React18 将当前组件的 Suspence 的值默认设置为 null 或 undefined。

```javascript
// React 17
const App = () => {
  return (
    <Suspense fallback={<Loading />}> // <--- 这个边界被使用，显示 Loading 组件
      <Suspense>                      // <--- 这个边界被跳过，没有 fallback 属性
        <Page />
      </Suspense>
    </Suspense>
  );
};
// React 18
const App = () => {
  return (
    <Suspense fallback={<Loading />}> // <--- 不使用
      <Suspense>                      // <--- 这个边界被使用，将 fallback 渲染为 null
        <Page />
      </Suspense>
    </Suspense>
  );
};
export default App;

```

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

<img src="../img/react事件机制.png" alt="react事件机制"  />



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



## 虚拟DOM的理解？

将页面状态抽象为js对象，合并多次dom修改结果，减少渲染次数。每渲染一次，虚拟dom都会缓存一次进行diff比较，决定渲染范围。



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



## hooks的实现原理

##### 以useState和useEffect为例：

##### react 渲染流程分为 render 和 commit 阶段。

render 阶段执行 vdom 转 fiber 的 reconcile，commit 阶段更新 dom，执行 effect 等副作用逻辑。

commit 阶段分为 before mutation、mutation、layout 3 个小阶段。

hook 的数据就是保存在 fiber.memoizedState 的链表上的，每个 hook 对应一个链表节点。

hook 的执行分为 mountXxx 和 updateXxx 两个阶段，第一次会走 mountXxx，创建 hook 链表，之后执行 updateXxx。



我们看了 useRef、useMemo、useCallback 的实现原理，这几个 hook 都比较简单。其中后两个 hook 是作为 props 时为了减少不必要渲染的时候用的。



useState 和 useEffect 就和渲染流程有关了：

useEffect 在 render 阶段会把 effect 放到 fiber.updateQueue 的环形链表上，然后在 commit 阶段遍历所有 fiber 的 updateQueue，取出 effect 异步执行。

useLayoutEffect 和 useEffect 差不多，只是 effect 链表是在 layout 阶段同步执行的。

useState 的 mountState 阶段返回的 setXxx 是绑定了几个参数的 dispatch 函数。执行它会创建  hook.queue 记录更新，然后标记从当前到根节点的 fiber 的 lanes 和 childLanes 需要更新，然后调度下次渲染。

下次渲染执行到 updateState 阶段会取出 hook.queue，根据优先级确定最终的 state，最后返回来渲染。

这样就实现了 state 的更新和重新渲染。



这就是 react hooks 特别是 useState 和 useEffect 的实现原理。理解它们不单单要理解 hook 的存储结构，还要理解 react 的整个渲染流程。



#### `useMemo` 和 `useCallback`区别

`useMemo` 和 `useCallback` 都是用于性能优化的 React Hook，它们的作用有所不同。

`useMemo` 用于缓存计算结果，避免重复计算。它接受两个参数：第一个参数是需要缓存的函数，第二个参数是依赖项数组。当依赖项数组中的任何一个值发生变化时，`useMemo` 会重新计算函数并返回新的结果；否则，`useMemo` 会返回上一次计算得到的结果。

`useCallback` 用于缓存函数实例，避免在每次重新渲染组件时创建新的函数。它也接受两个参数：第一个参数是需要缓存的函数，第二个参数是依赖项数组。当依赖项数组中的任何一个值发生变化时，`useCallback` 会创建一个新的函数实例；否则，`useCallback` 会返回上一次创建的函数实例。

虽然两者的作用类似，但它们的应用场景不同。`useMemo` 适用于需要进行大量计算的场景，如处理复杂的数据结构、执行复杂的算法等；而 `useCallback` 适用于需要传递给子组件的函数，避免在每次父组件重新渲染时创建新的函数实例。



## Fiber架构

#### 什么是Fiber，Fiber解决了什么问题？

**在React16以前，更新是通过树的深度优先遍历完成的**，遍历无法中断，当树的层级过深会产生栈的层级过深，页面渲染速度变慢。

故引入了Fiber，**React Fiber就是虚拟DOM，他是一个链表，返回return、children、siblings**，分别代表父Fiber，子Fiber和兄弟Fiber。

**Fiber是纤程，比线程更精细，表示对渲染线程实现更精细的控制，具备可中断、可恢复的特性。**



Fiber 架构通过将 React 应用拆分成一个个独立的单元（fiber），使得 React 应用可以被切片处理，这样就实现了对渲染进程的分割管理，同时也支持了在渲染过程中的中断和恢复。

Fiber 架构的核心是 Fiber 数据结构，每个 Fiber 实例都表示了应用中的一个组件或 DOM 节点。Fiber 节点包含了该节点的状态和生命周期方法，并且还能够记录子节点、兄弟节点等信息，这些信息都被保存在 Fiber 树中。

Fiber 架构采用了一种双缓存技术，通过使用两棵 Fiber 树来实现渲染过程的交替进行。在更新过程中，React 会先创建一颗新的 Fiber 树，并在树上标记需要更新的节点。然后 React 使用这颗新的 Fiber 树来计算出需要更新哪些 DOM 节点，并将这些更新同步到浏览器中。

为了实现渐进式渲染和对用户输入的响应，Fiber 架构引入了优先级的概念。React 将更新分为不同的优先级，每个优先级对应着一种任务类型和时间限制。在高优先级任务到来时，React 会中断当前渲染任务，处理高优先级任务，并在恰当的时候返回到之前的任务上继续执行。

#### 应用目的

实现增量渲染，即是把一个渲染任务分解为多个渲染任务，具备可中断、可恢复的特性，按照优先级处理任务，达到更顺滑的用户体验。

#### **Fiber的可中断、可恢复怎么实现的**

Fiber是协程，比线程更小的单元，可以人为中断和恢复。因此我们通过Fiber把浏览器渲染分段执行，每执行一段时间就让出主线程控制权，执行优先级更高的任务。

Fiber是链表结构，有三个指针，分别记录当前节点的下一个兄弟节点、子节点、父节点。当遍历中断时，只需要保留当前节点的索引，就能根据索引找到对应的节点，恢复遍历。

#### **Fiber更新机制**

基于协调算法、Fiber 数据结构、双缓存技术和优先级控制等多种新特性，实现了更加灵活、可中断和高效的更新过程。

在新建的alternate树上，完成整个子节点的遍历，包括Fiber的创建，最后会以workInProgress树为最新的渲染树，FiberRoot的current指针指向workInProgress使其变成current Fiber，完成初始化流程。



#### 双缓冲模式

React的current树和workInProgress树使用双缓冲模式，可以减少Fiber节点的开销，减少性能损耗。



#### React渲染流程

如图，React用JSX描述页面，经过babel编译为render function，执行后产生VDOM，VDOM不是直接渲染的，会先转换为Fiber，再进行渲染。vdom转换为Fiber的过程叫reconcile，转换过程会创建DOM，全部转换完成后会一次性commit到DOM,这个过程不是一次性的，是可打断的。

React 的渲染流程可以概括为以下几个步骤：

1. 构建组件树

React 应用由多个组件构成，每个组件都有自己的状态和属性（props）。在渲染过程中，首先需要构建组件树，也就是创建每个组件的实例，并将它们按照层次结构连接起来，形成完整的组件树。

1. 计算组件更新

一旦组件树构建完成，React 会通过 diff 算法比较前后两次渲染的组件树，以确定哪些组件需要进行更新。对于需要更新的组件，React 会调用它的生命周期方法和 setState 方法，计算出它的新状态和属性值，并将这些值保存到组件实例中。

1. 生成 Virtual DOM

在计算出每个组件的最新状态和属性后，React 会使用这些值生成一个虚拟 DOM 树，也就是 Virtual DOM。Virtual DOM 是 React 自己实现的一种轻量级的文档对象模型，它与浏览器的 DOM 类似，但是更加高效，并且能够方便地进行 diff 操作。

1. Diff 算法优化

React 通过 diff 算法比较前后两次渲染的 Virtual DOM，找出需要更新的部分，并尽可能地复用已经存在的 DOM 节点，从而减少 DOM 操作的次数，提高渲染效率。React 会使用一些优化策略，例如键（key）检查、分层比较等，以尽可能地加速 diff 过程。

1. 渲染到真实 DOM

最后，React 将更新后的 Virtual DOM 渲染到浏览器中，并对需要更新的 DOM 节点进行操作。在渲染过程中，React 会使用一些技巧，例如批量更新、异步渲染等，以提高性能并保持用户界面的响应性。



#### react的diff算法

React 的 diff 算法是为了在组件树发生变化时，尽可能地减少 DOM 操作的次数，并且保持高性能和响应性。它是 React 中最核心的算法之一，也是其高效渲染的关键所在。

React 的 diff 算法会对比前后两次渲染的 Virtual DOM 树，找出需要更新的部分，并尽可能地复用已存在的节点，以减少 DOM 操作的次数。以下是 React diff 算法的基本规则：

1. 同级比较：React 只会比较同层级的节点，不会跨层级进行比较。
2. 不同类型的节点：如果前后两个节点的类型不同，React 会直接删除旧节点，并创建新节点并插入到相应位置。
3. 相同类型的节点：如果前后两个节点的类型相同，则 React 会比较它们的属性和子节点，来判断是否需要更新该节点。
4. 使用 key 进行优化：在比较子节点时，React 会使用子节点的 key 属性来帮助确定它们之间的对应关系。这样可以尽量减少删除、创建和移动节点的操作，提高算法效率。
5. Diff 策略：在比较子节点时，React 会采用不同的 diff 策略，例如只向后遍历、逐层比较等，以尽可能地减少比较的次数。

