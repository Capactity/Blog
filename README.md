# blog

## JavaScript 相关

- [JavaScript 常用的设计模式](https://github.com/Capactity/blog/blob/master/javascript/JavaScript常用的设计模式.md)
- [JavaScript 原型与原型链的理解](https://github.com/Capactity/blog/blob/master/javascript/JavaScript原型与原型链的理解.md)
- [EventLoop 设计原理](https://github.com/Capactity/blog/blob/master/javascript/EventLoop设计原理.md)
- [Promise 实现](https://github.com/Capactity/Blog/blob/main/javascript/Pormise.md)
- JavaScript 抽象语法树 AST
- JavaScript 混淆安全

## 计算机网络

- [从输入 url 到页面显示的全过程](https://github.com/Capactity/Blog/blob/main/network/从输入url到页面显示的全过程.md)
- https 相关
- websocket 协议详解

## 浏览器

- 页面渲染卡顿原因/内存泄漏
- CSRF 攻击/XSS 攻击

## 前端框架

- [前端路由实现方式](https://github.com/Capactity/Blog/blob/main/frontend/前端路由实现方式.md)
- [React](https://github.com/Capactity/Blog/blob/main/frontend/react.md)
- [微前端实践](https://github.com/Capactity/Blog/blob/main/frontend/微前端.md)

## 打包工具

- [Webpack 配置过程](<(https://github.com/Capactity/Blog/blob/main/bundle-tool/webpack配置过程.md)>)
- Webpack 构建和性能优化探索
- vite 原理分析

## 数据结构&算法

### 数据结构

> 线性结构：数组、链表、队列和栈。

> 非线性结构：哈希表、树和二叉树。

- [数组](https://github.com/Capactity/blog/blob/master/data-structure/数组.md)
- [链表](https://github.com/Capactity/blog/blob/master/data-structure/链表.md)
- [栈](https://github.com/Capactity/blog/blob/master/data-structure/栈.md)
- [队列](https://github.com/Capactity/blog/blob/master/data-structure/队列.md)
- [堆](https://github.com/Capactity/blog/blob/master/data-structure/堆.md)
- [哈希表](https://github.com/Capactity/blog/blob/master/data-structure/哈希表.md)
- [二叉树](https://github.com/Capactity/blog/blob/master/data-structure/二叉树.md)

### 算法

- [数组](https://github.com/Capactity/blog/blob/master/algorithm/array/array.md)
- [链表]
- [哈希表]
- [字符串]
- [双指针]
- [栈与队列]
- [二叉树]
- [动态规划](https://github.com/Capactity/blog/blob/master/algorithm/动态规划/index.md)
- [刷题心得](https://github.com/sisterAn/JavaScript-Algorithms)

## 前端综合

- [浏览器工作原理]

## 可视化基础

### canvas

[Canvas_API](https://github.com/Capactity/blog/blob/master/Canvas_API.md)

### d3

### 可视化问题汇总

[根据鼠标位置缩放图形](https://github.com/Capactity/blog/blob/master/canvas/鼠标位置缩放图形.md)

[canvas 拖拽图形]

[局部渲染实现方案]

[canvas 获取点击选中节点最优算法]

[性能优化注意点](https://zhuanlan.zhihu.com/p/452871005)

11.减少 canvas 指令的调用可以提高性能，减少状态机的更新一样可以的，比如绘制 1000 个点，假设 1000 个点有 10 中颜色，可以根据颜色对图形进行分组，减少 fillStyle 等状态的变更（先把同一种颜色的绘制完，接着另一种颜色绘制），而不是每次都去改变 fillStyle,当然其他的状态(strokeStyle 等)也是一样的 2.坐标可以取整，浮点坐标会导致 canvas 进行抗锯齿处理

3.clip 最好少用，clip 是个很昂贵的操作，局部渲染用 bbox 擦除和重绘即可
