# webpack配置过程

> webpack的五大核心概念的分别是：`Entry`（入口）、`Output`（出口）、`Loader`（翻译器）、`Plugins`（插件）、`Mode`（模式）

webpack的构建流程包括**初始化**、**构建阶段**、**生成阶段**。



## 核心概念

#### 1 Entry

入口起点，告诉webpack应用应该使用哪个模块作为构建其内部依赖的开始，默认是**.src/index.js**，可以通过**webpack.config.js**文件中entry属性来配置，可指定一个或多个入口起点

```js
// 单文件入口完整写法
module.exports = {
  entry: {
    main: './src/index.js',
  },
};
 
// 多文件入口写法
module.exports = {
  entry: ['./src/index.js', './src/index_2.js'],
  output: {
    filename: 'bundle.js',
  },
};
```

#### 2 Output

output告诉webpack在哪输出创建的bundle，以及如何命名。默认是./dist/main.js，其他生成文件默认放置在./dist文件夹，，可通过在**webpack.config.js**文件中output属性来配置

```js
const path = require('path');
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
  },
};
```

#### 3 Loader

webpack只能理解JS和JSON文件，loader通过转换其他类型的文件，使webpack能够识别这些有效模块。**loader的执行顺序从上到下**。loader有两个基本属性：

- **test：识别出哪些文件会被转换**
- **use：定义在进行转换时，使用哪个loader**

```js
// 示例
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // 最后执行 style-loader
          { loader: 'style-loader' },
          // 其次执行 css-loader
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          //  首先执行 sass-loader
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

#### 4 Plugins

用于执行大部分范围的任务，包括：打包优化、资源管理、注入环境变量等。

插件使用需要require()引入，然后通过new操作符来创建实例最后添加到plugins数组中。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // html的插件
 
module.exports = {
  module: {
    rules: [{ test: /\.css$/, use: 'css-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

#### 5 Mode

有生产模式（production）和开发模式（development）或none。设置mode参数，可以启用webpack内置在相应环境下的优化

```js
// 其默认值为 production
module.exports = {
  mode: 'production',
};
```



## 构建流程

### 初始化阶段

#### 1 初始化参数

从配置文件`webpack.config.js`和`shell`语句中读取参数，获取合并后的参数。

#### 2 创建编译器对象

获取参数，初始化`Compiler`对象。

#### 3 初始化编译环境

遍历定义的plugins数组，执行插件的apply方法。加载webpack的内置插件等，生成Compiler实例，配置完对应的环境参数。

#### 4 开始编译

执行`Compiler`对象的`run`方法

#### 5 确定入口

根据配置中的 `entry` 找出所有的入口文件，调用 `compilation.addEntry` 将入口文件转换为 `dependence` 对象



### 构建阶段

该阶段主要围绕module进行，如下：

#### 1 编译模块（make）

#### 2 完成模块编译

### 生成阶段

#### 1 输出资源（seal）

#### 2 写入文件系统（emitAssets）

