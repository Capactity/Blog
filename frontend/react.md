## react18有哪些更新

#### 1 setState自动批处理

react17中，只有react事件会进行批处理，原生j s事件，promise、setTimeout、setInterval等则不会。

**react18中，将所有事件都进行批处理，即多次setState会被合并为1次执行，提高了性能**。在数据层，将多次状态更新合并成一次处理（在视图层，将多次渲染合并成一次渲染）。



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

**当使用严格模式时，react对每个组件返回两次渲染**，在react17中去掉了一次渲染的控制台日志。react18取消了这个限制，第二次渲染会以浅灰色出现在控制台日志。



4 Suspence不再需要fallback捕获。



5 Concurrent Mode
