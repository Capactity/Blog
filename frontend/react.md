## React18有哪些更新

#### 1 setState自动批处理

React17中，只有react事件会进行批处理，原生j s事件，promise、setTimeout、setInterval等则不会。

**React18中，将所有事件都进行批处理，即多次setState会被合并为1次执行，提高了性能**。在数据层，将多次状态更新合并成一次处理（在视图层，将多次渲染合并成一次渲染）。



#### 2 引入新 root API,支持new concurrent renderer（并发模式的渲染） 

```javascript
//React 17
import React from "react"
import ReactDOM from "react-dom"
import App from "./App"

const root = document.getElementById("root")
ReactDOM.render(root,<App/>)

// 卸载组件
ReactDOM.unmountComponentAtNode(root)  

// React 18
import React from "react"
import ReactDOM from "react-dom/client"
import App from "./App"
const root = document.getElementById("root")
ReactDOM.createRoot(root).render(<App/>)

// 卸载组件
root.unmount()
```



#### 3 strict mode更新

**当使用严格模式时，React对每个组件返回两次渲染**，在React17中去掉了一次渲染的控制台日志。React18取消了这个限制，第二次渲染会以浅灰色出现在控制台日志。



#### 4 React18在render中删除了回调函数，如果需要使用回调，可以通过useEffect实现。

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

它可以帮助应用保持响应。通过渲染可终端来修复阻塞渲染机制。在并发模式中，react可以同时更新多个状态，**使同步不可中断更新变成了异步可中断更新**。



#### 6 Suspence不再需要fallback捕获

**Suspence的作用是划分页面中需要并发渲染的部分，和lazy一起使用服务于优化打包，可以显著减少主包体积，提升用户体验。**

React17中，如果Suspence没有提供fallback属性，React会跳过继续向上搜索下一个边界，这可能导致难以调试。比如debug在没有fallback的Suspence组件中抛出一个边界问题进行测试，不会触发警告报错。

React18将当前组件的Suspence的值默认设置为null或undefined。

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



## React设计思想

- 组件化 - 组件符合开闭原则，渲染工作流封闭，组件通信通过props进行数据交互开放。
- 数据驱动视图 - UI = f(data) 通过修改数据（state或prop），数据驱动视图更新。
- 虚拟DOM - 是对真实DOM的映射，通过新旧虚拟DOM对比，得到需要更新部分，实现数据增量更新。



## React生命周期

组件生命周期分为**挂载**、**更新**、**卸载**阶段

**挂载：**

- constructor 进行state和props的初始化
- render
- componentDidMount 第一次渲染后调用，可以访问DOM进行异步请求和定时器、消息订阅。

**更新：**

- 

