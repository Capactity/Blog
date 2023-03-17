## canvas性能优化方向

https://juejin.cn/post/7135229172409958431









## canvas API





























## 绘制多边形


1. beginPath() 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
2. moveTo(x1, y1) 指定起始坐标点。
3. lineTo(x2, y2) 添加一个新点(x, y)，然后创建从当前点(x1, y1)或(xn, yn)到目标点(x2, y2)的线条。
4. closePath() 闭合路径，闭合路径之后图形命令又重新指向到上下文中。
5. stroke() 通过线条来绘制图形轮廓。
6. fill() 通过填充路径的内容区域生成实心图形。

当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。

[绘制多边形 DEMO](https://jsfiddle.net/caiwei8/1kn3esha/)



## 矩形

1. fillRect(x, y, width, height) 填充矩形。
2. strokeRect(x, y, width, height) 绘制矩形边框。
3. clearRect(x, y, width, height) 清除指定矩形区域，让清除部分完全透明。
4. rect(x, y, width, height) 绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。

[绘制矩形 DEMO](https://jsfiddle.net/caiwei8/gsL78jhd/)



## 设置颜色

1. 基本颜色
   - fillStyle = color 设置图形的填充颜色。
   - strokeStyle = color 设置图形轮廓的颜色。
2. 渐变颜色
   - 线性渐变，创建一个水平、垂直或对角线的填充图案。
   - 径向渐变，创建一个放射状的填充。

填充渐变一般分三步，添加渐变线，为渐变线添加关键色，应用渐变。

color支持使用 rgba 及 hsla 形式。

[fillStyle DEMO](https://jsfiddle.net/caiwei8/hn2p574v/)
[strokeStyle DEMO](https://jsfiddle.net/caiwei8/fj45no3L/)

[线性渐变 DEMO](https://jsfiddle.net/caiwei8/v0yeubz7/)

[径向渐变 DEMO](https://jsfiddle.net/caiwei8/hn2p574v/)

## 绘制圆弧





## 二次贝塞尔曲线





## 三次贝塞尔曲线





## N次贝塞尔曲线




## 图形旋转与翻转



## 已知连线角度及其中一个坐标，求另一坐标



## 坐标旋转与坐标系转换



## 文本渲染





## 阴影

