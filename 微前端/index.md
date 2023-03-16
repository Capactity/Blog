## monorepo 模式与隐式依赖

monorepo 模式目前也用得越来越多，我个人也很喜欢这种模式，而且我还喜欢将各个子包的依赖描述都定义在**根 package.json** 中，因为这样在各个 package 中可以自由、方便地使用依赖，但这实际上是一个误区行为。

在 monorepo 模式中，无论是 lerna 还是 yarn 工作机制的核心都是：

- 将所有 package 的依赖都尽量以 flat 模式安装到根级别的 node_modules 里，即 host，以避免各个 package 重复安装第三方依赖；将有冲突的依赖，安装在各自 package 的 node_modules 里，以解决依赖的版本冲突问题。
- 将各个 package 都软链到根级别的 node_modules 里，这样各个 package 利用 Node 的递归查找机制，可以导入其他 package，不需要自己进行手动的 link。
- 将各个 package 里 node_modules 的 bin 软链到 root level 的 node_modules 里，保证每个 package 的 npm script 能正常运行。

但是

- packageA 可以轻松的导入 packageB，即使没有在 packageA 里声明 packageB 为其依赖，甚者 packageA 可以轻松地导入 packageB 的第三方依赖，类似上述的误区行为。

因为这样一来，实际上将是隐式依赖的问题加剧放大了，所以使用的时候还是需要稍加注意。



