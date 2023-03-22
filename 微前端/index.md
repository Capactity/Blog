## monorepo 模式与隐式依赖

monorepo 模式目前也用得越来越多，我个人也很喜欢这种模式，而且我还喜欢将各个子包的依赖描述都定义在**根 package.json** 中，因为这样在各个 package 中可以自由、方便地使用依赖，但这实际上是一个误区行为。

在 monorepo 模式中，无论是 lerna 还是 yarn 工作机制的核心都是：

- 将所有 package 的依赖都尽量以 flat 模式安装到根级别的 node_modules 里，即 hoist，以避免各个 package 重复安装第三方依赖；将有冲突的依赖，安装在各自 package 的 node_modules 里，以解决依赖的版本冲突问题。
- 将各个 package 都软链到根级别的 node_modules 里，这样各个 package 利用 Node 的递归查找机制，可以导入其他 package，不需要自己进行手动的 link。
- 将各个 package 里 node_modules 的 bin 软链到 root level 的 node_modules 里，保证每个 package 的 npm script 能正常运行。

但是

- packageA 可以轻松的导入 packageB，即使没有在 packageA 里声明 packageB 为其依赖，甚者 packageA 可以轻松地导入 packageB 的第三方依赖，类似上述的误区行为。

因为这样一来，实际上将是隐式依赖的问题加剧放大了，所以使用的时候还是需要稍加注意。



## monorepo能解决什么问题？

 减少了维护和管理多个代码库的成本，具备以下优点：

1. **代码复用：**多个项目共享一个代码库，避免不同项目重复编写功能
2. **提升协效率：**同一代码库，方便共享文档和代码
3. 集中管理：不同应用程序在同一代码库中，方便管理监控
4. **统一构建：**共用一套工具链进行架构部署，提升构建效率
5. **一个版本：**无需担心因为项目以来于第三方库的冲突版本而导致的不兼容问题。



#### 缺点

仓库体积更大，统一构建工具，对构建工具要求更高（模块联邦）



## Monorepo 如何搭建？

最简单的方式是采用 pnpm + workspace 来搭建，不需要其他额外的工具，利用 pnpm 提供 filter 命令，基本可满足大部分场景。

搭建方式也很简单可以

```
├── packages
|   ├── pkg1
|   |   ├── package.json
|   ├── pkg2
|   |   ├── package.json
├── package.json
├── pnpm-workspace.yaml
```

在 pnpm-workspace.yaml 中指定 packages 的入口

```yaml
packages:
  # all packages in subdirs of packages/ and components/
  - 'packages/**'
```

在 pkg1 中引用 pkg2 只需要在 pkg1 的 package.json 中的依赖中进行添加

```json
"dependencies": { 
    "@test/utils": "workspace:^1.0.0"
}
```

在根的 package.json 中通过 filter 配置 script 来指定特定的项目执行

```json
"build:pkg1": "pnpm --filter=@pkg1 build",
```

当你的项目需要根据 package.json 下的依赖进行拓扑排序并行打包、增量打包、打包缓存、依赖图可视化等等各种功能时，你可以考虑一些构建工具



## 微前端的应用通信方案？

1. 基于URL通信，能力较弱，但是有些简单场景还是很好用的，例如主应用需要识别渲染哪个子应用可以在URL地址栏加上子应用id，当子应用内部有一个跳转逻辑的时候直接修改这个参数就可以了
2. 基于storage，这种方式虽然可以互通某些数据，但是不能做到实时相应
3. 发布订阅模型，利用自定义事件实现一套通信机制
4. 基于props传参的模式，所有微应用与主应用通信，主应用再通知微应用，这种方式耦合性比较强

![img](https://pic4.zhimg.com/80/v2-54f479648cb64932eb0d5d449cec714f_1440w.webp)

## 微前端如何进行安全隔离？

微前端中的安全隔离分为样式隔离和脚本隔离；如果使用了全局样式那么主应用与子应用之间可能产生样式污染；js脚本则主要是全局变量的污染；

样式隔离有以下方案：

1. 动态样式表，应用卸载之后样式也同时卸载；这种方式需要注意的问题是微应用卸载时也要通知主应用将微应用的样式卸载掉，如果卸载不成功则会造成样式污染
2. 工程化手段：css module，css in js，无法杜绝项目中使用全局样式，可能需要我们约定只能在主应用中修改全局组件样式
3. shadow DOM，天然的样式隔离
4. 运行时样式转换：运行时给所有节点增加一个属性，利用属性选择器创建命名空间div[data-qiankun-subvue1]
5. BEM约束，或者编译时增加namespace



js隔离

1. 存储js变更的快照，等到应用卸载之后还原快照，例如Qiankun
2. 借助iframe实现，例如各大线上编辑器都是用iframe嵌入html页面
3. 借助Worker

Worker自然拥有js沙箱环境，但是Worker中无法访问dom对象，所以我们可以通过编译“注入”document上下文，也就是说我们在Worker中进行dom的操作最终会编译到主线程去操作，但是变量依然是寄存在Worker中；当前还没有很好的框架去封装这个功能，让我们期待一下；



