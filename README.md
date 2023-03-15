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
- [基于 qiankun 的微前端最佳实践](https://juejin.cn/post/6844904151231496200)
- [微前端实践](https://github.com/Capactity/Blog/blob/main/frontend/微前端.md)

## 打包工具

- [Webpack 配置过程](<(https://github.com/Capactity/Blog/blob/main/bundle-tool/webpack配置过程.md)>)
- Webpack 构建和性能优化探索
- vite 原理分析

## 数据结构&算法

### 数据结构

> 线性结构：数组、链表、队列和栈 。

> 非线性结构：哈希表、树和二叉树。

- [数组](https://github.com/Capactity/blog/blob/master/data-structure/数组.md)
- [链表](https://github.com/Capactity/blog/blob/master/data-structure/链表.md)
- [栈](https://github.com/Capactity/blog/blob/master/data-structure/栈.md)
- [队列](https://github.com/Capactity/blog/blob/master/data-structure/队列.md)
- [堆](https://github.com/Capactity/blog/blob/master/data-structure/堆.md)
- [哈希表](https://github.com/Capactity/blog/blob/master/data-structure/哈希表.md)
- [二叉树](https://github.com/Capactity/blog/blob/master/data-structure/二叉树.md)

### 算法

- [数组专题](https://github.com/Capactity/blog/blob/master/algorithm/array/array.md)
- [链表专题]
- [哈希表专题](https://github.com/Capactity/blog/blob/master/algorithm/哈希表/index.md)
- [字符串专题]
- [双指针专题]
- [栈与队列专题]
- [二叉树专题](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/index.md)
- [回溯算法](https://github.com/Capactity/blog/blob/master/algorithm/回溯算法/index.md)
- [贪心算法](https://github.com/Capactity/blog/blob/master/algorithm/贪心算法/index.md)
- [动态规划](https://github.com/Capactity/blog/blob/master/algorithm/动态规划/index.md)
- [刷题心得](https://github.com/sisterAn/JavaScript-Algorithms)

## 前端综合

- [浏览器工作原理]
- [微前端](https://github.com/Capactity/blog/blob/master/微前端/index.md)
- [webpack基础](https://github.com/Capactity/blog/blob/master/webpack基础/index.md)

## 可视化基础

- [图形学基础](https://juejin.cn/post/6912086785405386765#comment)
- [Three.js基础](https://github.com/Capactity/blog/blob/master/可视化/threejs.md)

### canvas

[Canvas_API](https://github.com/Capactity/blog/blob/master/Canvas_API.md)

### d3

### 可视化问题汇总

[根据鼠标位置缩放图形](https://github.com/Capactity/blog/blob/master/canvas/鼠标位置缩放图形.md)

[canvas 拖拽图形]

[局部渲染实现方案]

> - clip() 确定绘制的的裁剪区域，区域之外的图形不能绘制，详情查看 [CanvasRenderingContext2D.clip()](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FCanvasRenderingContext2D%2Fclip)
> - clearRect(*x*, *y*, *width*, *height*) 擦除指定矩形内的颜色，查看 [CanvasRenderingContext2D.clearRect()](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FCanvasRenderingContext2D%2FclearRect)

通过API可得到局部刷新方案：

1. 清除指定区域的颜色，并设置 clip
2. 所有同这个区域相交的图形重新绘制

1 bbox擦除后重绘  

2 clip重绘有明显的性能问题



> canvas 渲染较大画布的时候性能会较低？为什么？

因为 canvas 依赖于像素，在绘制过程中是一个一个像素去绘制的，当画布足够大，像素点也就会足够多，那么想能就会足够低。

> 假设现在有 5000 个圆，完全绘制出来，点击某一个圆，该圆高亮，另外 4999 个圆设为半透明，分别说说用 svg 和 canvas 怎么实现？

首先，从数据出发，我们的每个圆是一个数据，这个数据有圆的 x、y、radius�、isHighlight 如果是 svg，直接渲染节点即可，然后往节点上边绑定点击事件，点击改变所有数据的高亮属性（必须同步执行完成），然后让浏览器进行绘制。如果是 canvas，我们需要自己绑定事件到 canvans 标签上，然后点击的时候判断点击的位置是否在圆内，如果在某个圆内，则更新所有数据的高亮属性，之后在进行一次性绘制。

> canvas 的点击事件，怎么样实现？假如不是圆，这些图形是正方形、长方形、规则图形、不规则图形呢。

针对于每一个形状，将其抽象成 shape 类，每一个类有自己的方法 isPointInSide 来判断节点是否在图形内，对于不规则图形，当做矩形处理，点击的时候执行该方法判断点击位置是否在图形内。

> 假如图形可能有变形、放大、偏移、旋转的需求？这个 isPointInSide 怎么处理？

有相应的 API 处理变形、旋转、放大等等之后的位置映射关系。

[canvas 获取点击选中节点最优算法]

> 这个 canvas 的点击事件，点击的时候怎么样快速的从这 5000 个圆中找到你点击的那个圆（不完全遍历 5000 个节点）？

可以通过预查找的形式，当鼠标划过的时候预先查找到鼠标附近的一些节点，当点击的时候在从这些预先筛选好的节点里查找点击下来的节点，当然这个方法的前提是不能影响 js 主线程的执行，必须是异步的形式。

[性能优化注意点](https://zhuanlan.zhihu.com/p/452871005)

11.减少 canvas 指令的调用可以提高性能，减少状态机的更新一样可以的，比如绘制 1000 个点，假设 1000 个点有 10 中颜色，可以根据颜色对图形进行分组，减少 fillStyle 等状态的变更（先把同一种颜色的绘制完，接着另一种颜色绘制），而不是每次都去改变 fillStyle,当然其他的状态(strokeStyle 等)也是一样的 2.坐标可以取整，浮点坐标会导致 canvas 进行抗锯齿处理

3.clip 最好少用，clip 是个很昂贵的操作，局部渲染用 bbox 擦除和重绘即可
