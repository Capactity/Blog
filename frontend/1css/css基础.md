## CSS基础

#### **1 CSS选择器及其优先级**

对于选择器的优先级：

- 标签选择器、伪元素选择器：1；
- 类选择器、伪类选择器、属性选择器：10；
- id 选择器：100；
- 内联样式：1000；
- !important：10000；





#### **transition和animation**区别

```vbnet
transition: property duration timing-function delay;
1
```

- property：指定CSS属性的name，transition效果
- duration：transition效果需要指定多少秒或毫秒才能完成
- timing-function：指定transition效果的转速曲线，默认是 “ease”
- delay：定义transition效果开始的时候

```arduino
.box{ 
   animation: name duration timing-function delay iteration-count direction fill-mode play-state;
}
123
```

- name：规定 @keyframes 动画的名称。
- duration：动画指定需要多少秒或毫秒完成
- timing-function： 规定动画的速度曲线，默认是 “ease”
- delay：设置动画在启动前的延迟间隔，默认是 0
- iteration-count：定义动画的播放次数(infinite为循环播放)（默认为1）
- direction：指定是否应该轮流反向播放动画，默认是 “normal”
- fill-mode：规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式
- play-state：指定动画是否正在运行或已暂停，默认是 “running”

transition需要触发一个事件才能改变属性，而animation不需要触发任何事件就会随时间改变属性值transition为两帧，而animation可以是一帧一帧的，跟随自定义动画而言，自定义动画定义了多少帧就执行多少帧

animation 可以设置很多的属性，比如循环次数，动画结束的状态等等，transition 只能触发一次



#### 2 **对盒模型的理解**

盒模型都是由四个部分组成的，分别是margin、border、padding和content。

标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同：
1标准盒模型的width和height属性的范围只包含了content，
2IE盒模型的width和height属性的范围包含了border、padding和content。

可以通过修改元素的box-sizing属性来改变元素的盒模型：
1box-sizing: content-box表示标准盒模型（默认值）
2box-sizing: border-box表示IE盒模型（怪异盒模型）



#### 4 Sass、Less 是什么？为什么要使用他们？ 

他们都是 CSS 预处理器，是 CSS 上的一种抽象层。他们是一种特殊的语法/语言编译成 CSS。 例如 Less 是一种动态样式语言，将 CSS 赋予了动态语言的特性，如变量，继承，运算， 函数，LESS 既可以在客户端上运行 (支持 IE 6+, Webkit, Firefox)，也可以在服务端运行 (借助 Node.js)。 

为什么要使用它们？ 
1 结构清晰，便于扩展。 可以方便地屏蔽浏览器私有语法差异。封装对浏览器语法差异的重复处理， 减少无意义的机械劳动。 
2 可以轻松实现多重继承。 完全兼容 CSS 代码，可以方便地应用到老项目中。LESS 只是在 CSS 语法上做了扩展，所以老的 CSS 代码也可以与 LESS 代码一同编译。



#### 5 对 CSS 工程化的理解

CSS 工程化是为了解决以下问题：
1宏观设计：CSS 代码如何组织、如何拆分、模块结构怎样设计？
2编码优化：怎样写出更好的 CSS？
3构建：如何处理我的 CSS，才能让它的打包结果最优？
4可维护性：代码写完了，如何最小化它后续的变更成本？如何确保任何一个同事都能轻松接手？

以下三个方向都是时下比较流行的、普适性非常好的 CSS 工程化实践：
1 预处理器：Less、 Sass 等；
2 重要的工程化插件： PostCss；

1. PostCSS是一个平台，具体要取决于这个平台上面的插件可以做什么

2. 常用的插件如下

   ```js
    //  1. 可以添加属性前缀，适应所有的浏览器
        const autoprefixer = require('autoprefixer');
        // 2. 将所有的import 导入进来的模块全部合并为一个文件
        // const atImport = require('postcss-import');
        // 3.  实现代码的压缩优化
        // const cssnano = require('postcss-cssnano');
        // 4. cssnext提前使用CSS的高级语法
        // const cssnext = require('postcss-cssnext');
        // 5. precss 类似于sass的语法处理
        // const precss = require('precss')
   Copy
   ```

   1. import实现模块的合并（模块分开，提前合并）
   2. CSS语法检查，兼容性检查
   3. 压缩文件

3 Webpack loader 等 



#### 12 什么是margin重叠问题？如何解决？

问题描述：
两个块级元素的上外边距和下外边距可能会合并（折叠）为一个外边距，其大小会取其中外边距值大的那个，这种行为就是外边距折叠。需要注意的是，浮动的元素和绝对定位这种脱离文档流的元素的外边距不会折叠。重叠只会出现在垂直方向。

计算原则：
折叠合并后外边距的计算原则如下：
1 如果两者都是正数，那么就去最大者
2 如果是一正一负，就会正值减去负值的绝对值
3 两个都是负值时，用0减去两个中绝对值大的那个

解决办法：
对于折叠的情况，主要有两种：兄弟之间重叠和父子之间重叠
（1）兄弟之间重叠
1 底部元素变为行内盒子：display: inline-block
2 底部元素设置浮动：float
3 底部元素的position的值为absolute/fixed
（2）父子之间重叠
1 父元素加入：overflow: hidden
2 父元素添加透明边框：border:1px solid transparent
3 子元素变为行内盒子：display: inline-block
4 子元素加入浮动属性或定位





#### **前端动画实现方式**

1. css3的`transition` 属性
2. css3的`animation` 属性
3. 原生JS动画
4. 使用`canvas`绘制动画
5. SVG动画
6. 使用gif图片



