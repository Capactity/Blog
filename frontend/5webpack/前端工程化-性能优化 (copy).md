#### **对前端工程化的理解**

前端工程化的目的主要在于提高开发效率以及提高代码质量，增加项目的可维护性及降低后期更新迭代的难度。

**组件化**

组件化是把UI页面拆分为不同的部分，一个页面是由一个大组件构成，大组件又包含多个中型组件，中型组件又可以包含多个小组件。每个组件都是由html, css, js构成的完备单元。

**模块化**

模块化的工具有webpack

**规范化**

项目的目录结构是否合理，有没有对项目的资源做统一的定位管理。

通过代码规范，命名规范以及前后端接口数据规范，提高团队协作的能力及代码质量。

前后端分离开发，仅通过异步ajax交换数据，可以使前后端独立松耦合。



#### **webpack和vite的区别**

webpack **显式指定了模块对应的 loader**，正是这个机制，导致 vue SFC 的 script、template、style，没办法被其他 loader 处理，需要插件做一些复杂的操作，最终用 `Inline loader import` 强制指定 loader，整个过程比较复杂。

Vite/Rollup 的模块**会经过所有的插件**，在插件中过滤出需要处理的模块，其他的交给下一个插件处理。这样的机制使 Vue 文件的各个部分，能经过所有插件的处理，从而避免了 webpack 遇到的问题，这也使在 Vite/Rollup 中的转换实现更为清晰和简单。

最后，我们通过这样的对比，目的不能说明 Webpack/Vite/Rollup 谁好谁坏，而是在学习过程中，横向对比，加深对它们的了解。



目前已知 vite 的缺点有：

1. 热更新常常失败，原因不清楚

1. 有些功能 rollup 不支持，需要自己写 rollup 插件

1. 不支持非现代浏览器

   

#### webpack如何优化前端性能

- 压缩代码:删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的`UglifyJsPlugin`和`ParallelUglifyPlugin`来压缩JS文件， 利用`cssnano`（css-loader?minimize）来压缩css
- 利用CDN加速: 在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于`output`参数和各loader的`publicPath`参数来修改资源路径
- Tree Shaking: 将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数`--optimize-minimize`来实现
- Code Splitting: 将代码按路由维度或者组件分块(chunk),这样做到按需加载,同时可以充分利用浏览器缓存
- 提取公共第三方库: SplitChunksPlugin插件来进行公共模块抽取,利用浏览器缓存可以长期缓存这些无需频繁变动的公共代码



`充分利用缓存提升二次构建速度`：

- babel-loader 开启缓存
- terser-webpack-plugin 开启缓存
- 使用 cache-loader 或者 hard-source-webpack-plugin

```
Tree shaking
```

- 打包过程中检测工程中没有引用过的模块并进行标记，在资源压缩时将它们从最终的bundle中去掉(只能对ES6 Modlue生效) 开发中尽可能使用ES6 Module的模块，提高tree shaking效率
- 禁用 babel-loader 的模块依赖解析，否则 Webpack 接收到的就都是转换过的 CommonJS 形式的模块，无法进行 tree-shaking
- 使用 PurifyCSS(不在维护) 或者 uncss 去除无用 CSS 代码
  - purgecss-webpack-plugin 和 mini-css-extract-plugin配合使用(建议)

```
Scope hoisting
```

- 构建后的代码会存在大量闭包，造成体积增大，运行代码时创建的函数作用域变多，内存开销变大。Scope hoisting 将所有模块的代码按照引用顺序放在一个函数作用域里，然后适当的重命名一些变量以防止变量名冲突
- 必须是ES6的语法，因为有很多第三方库仍采用 CommonJS 语法，为了充分发挥 Scope hoisting 的作用，需要配置 mainFields 对第三方模块优先采用 jsnext:main 中指向的ES6模块化语法

```
动态Polyfill
```

- 建议采用 polyfill-service 只给用户返回需要的polyfill，社区维护。 (部分国内奇葩浏览器UA可能无法识别，但可以降级返回所需全部polyfill)



#### 前端模块化

前端模块化开发是指将一个复杂的前端应用程序划分成多个相互独立、具有一定功能的模块，以便于管理和维护。模块化开发可以提高代码复用性，降低代码耦合度，提高代码可维护性和可扩展性等优点。

在模块化开发中，常见的模块化标准有 CommonJS、AMD、CMD 和 ES6 Module。

- CommonJS：主要用于服务器端 JavaScript，采用同步加载的方式，在 Node.js 中得到广泛应用。使用 require() 来引入模块，使用 module.exports 或 exports 来导出模块。
- AMD（Asynchronous Module Definition）：采用异步加载的方式，适用于浏览器环境下的模块化开发。使用 define() 函数定义模块，使用 require() 函数来引入模块。
- CMD（Common Module Definition）：也采用异步加载的方式，与 AMD 类似，但是对模块的依赖关系处理方式不同。使用 define() 函数定义模块，使用 require() 函数来引入模块。
- ES6 Module：是 ES6 标准新增的模块化规范，支持异步加载和静态编译，是未来 JavaScript 模块化的趋势。使用 import 语句来引入模块，使用 export 语句来导出模块。

这些模块化标准都提供了一种封装代码的机制，使得代码可以更好地组织和管理。在实际开发中，我们可以根据具体项目需求选择不同的模块化标准进行开发。同时，配合打包工具如Webpack、Rollup等，可以将多个模块打包成一个或多个文件，以便于部署到生产环境中。







#### 前端性能优化手段

1. ##### 压缩和合并资源文件：

   使用工具对CSS、JS、图片等文件进行压缩，减少网络传输的大小。使用Webpack、Gulp等构建工具将多个文件合并为一个文件，减少HTTP请求次数。

压缩文件：

1. 去除多余空格和注释：可以使用工具如UglifyJS、Terser等来去除CSS、JS中的多余空格和注释，从而减少文件大小。
2. 图片压缩：可以使用工具如TinyPNG、Kraken.io等对图片进行压缩，以达到减小图片文件大小的目的。
3. 字体压缩：可以使用Webfont-Loaders、Fontmin等工具对字体文件进行压缩。

合并文件：

1. 使用Webpack：Webpack作为现代前端构建工具，支持将多个JS和CSS文件合并为一个文件，并且还支持代码分割和按需加载等功能，使得网站在加载时只需要请求一个文件就能够渲染页面，从而提高页面性能和用户体验。
2. 使用Gulp：Gulp是另外一个流行的前端构建工具，通过使用gulp-concat、gulp-uglify等插件，可以将多个CSS和JS文件合并成一个文件。
3. 使用CDN：如果使用了第三方库或框架，可以将其存放在CDN上，从而避免了将这些大型文件打包进自己的应用程序中。



1. ##### 使用CDN加速：

   在网站上引入CDN提供商的静态资源，如jQuery, Bootstrap等第三方库，并尽量将图片等静态资源存放在CDN上。这样能够利用CDN分布在全球多个地区的服务器来缓存静态资源，减少用户请求的响应时间。



1. ##### 使用缓存机制：

   使用浏览器缓存或服务端缓存技术来缓存数据。例如，使用HTTP头控制浏览器缓存，使用Memory Cache、Disk Cache、Service Worker等技术实现浏览器缓存；使用Redis、Memcached等服务端技术实现服务端缓存。

浏览器缓存是一种常见的缓存机制，可以帮助减少网络传输次数和提高网页加载速度。在使用浏览器缓存时，主要有两种方式：强缓存和协商缓存。

​      强缓存

强缓存是使用HTTP响应头中的Expires或Cache-Control字段来设置资源缓存时间，让客户端在一定时间内（比如10分钟）保留缓存副本，在此期间再次访问同一个资源时直接从缓存中获取。如果缓存时间未过期，则不会向服务器发送请求，直接从缓存中读取数据。

例如，设置缓存时间为10分钟：

```
Copy CodeCache-Control: max-age=600
```

​     协商缓存

当强缓存失效时，客户端会将缓存加入到请求头中并发送给服务器，服务器会判断缓存是否过期，并返回相应的状态码和响应头，告诉客户端该请求是否会命中缓存。如果命中缓存，则客户端直接从缓存中读取数据。

协商缓存需要使用Last-Modified和ETag等字段进行缓存控制，通过这些字段来判断当前资源是否重新生成，如果没有改变，则可以继续使用缓存。

例如，使用 Last-Modified 和 If-Modified-Since 验证缓存是否过期：

```
Copy CodeLast-Modified: Fri, 07 May 2021 01:02:03 GMT
```

客户端首次请求时，服务器会返回资源的 Last-Modified 值，客户端缓存该值。当客户端再次请求时，在请求头中添加 If-Modified-Since 字段，将前一次请求返回的 Last-Modified 值传递给服务器。服务器通过比较当前资源的 Last-Modified 和 If-Modified-Since 的值，来判断缓存是否过期，如果资源未修改，则返回304状态码，告诉客户端可以继续使用缓存。



1. ##### 懒加载：

   通过延迟加载不必要的资源，例如图片、视频等，只有在需要时才加载，从而避免了在页面初次加载时一次性加载大量的资源，减少首屏加载时间，提高用户体验。可以使用Intersection Observer API实现懒加载。

2. ##### 代码优化：

   编写高效的HTML、CSS和JavaScript代码，使用现代的框架和库，减少DOM操作和动画效果，尽量避免阻塞渲染的操作，如快速响应用户交互事件。可以使用Chrome DevTools等工具来诊断性能瓶颈，并进行针对性优化。

3. 减少HTTP请求次数：通过压缩和合并文件、使用CDN、使用缓存等方式，减少HTTP请求次数，提高页面的加载速度。同时也可以使用CSS Sprites、Base64等技术将多个小图片合并为一个大图或直接使用Base64编码的方式内嵌到HTML文档中，从而减少HTTP请求次数。

4. 使用SSR：采用服务器端渲染技术，将页面的HTML、CSS和JavaScript代码在服务器端生成，减轻客户端的工作负担，提高页面的加载速度。可以使用Vue、React等现代前端框架的服务端渲染功能实现SSR。



#### 前端web安全

作为前端开发人员，可以从以下几个方面来提高Web安全性：

1. 输入验证：在前端对用户输入的内容进行验证，确保不会危及网站安全。比如通过正则表达式检查输入是否符合要求，过滤掉可能存在漏洞的字符（如SQL注入攻击）。
2. 防御跨站点脚本攻击（XSS）：XSS是一种常见的攻击方式，攻击者通过在输入框中插入恶意代码，然后让其他用户看到这些代码，从而窃取他们的信息。可以使用浏览器内置的CSP（Content Security Policy）功能来防御XSS攻击。
3. 防御跨站请求伪造（CSRF）：CSRF攻击利用了已登录的用户的身份来完成一些未经授权的操作，可以通过在前端中加入随机Token来防御此类攻击。
4. HTTPS协议：使用HTTPS协议能够加密数据传输，防止中间人篡改和窃取数据。同时，也可以使用HSTS（HTTP Strict Transport Security）来强制浏览器只使用HTTPS。
5. 安全头部设置：在HTTP响应中添加一些安全头部，例如X-Content-Type-Options、X-XSS-Protection、Strict-Transport-Security等，可以帮助防止一些常见的攻击。
6. 升级依赖包版本：定期升级前端依赖包版本，保持与最新版本同步，避免被已知漏洞攻击。











#### 前端发布流程

前端发布流程一般包括以下几个步骤：

1. 开发阶段：在开发阶段，前端开发人员编写代码，并进行本地测试和调试。
2. 代码审查：在开发完成后，代码需要经过代码审查来确保代码的质量和一致性。代码审查可以由团队内的其他成员或专门的审核人员来完成。
3. 打包构建：代码审查通过后，需要将代码进行打包构建。打包构建是将所有的代码文件和依赖项打包到一个或多个压缩文件中，以便于上传和部署。
4. 测试环境部署：在代码打包构建成功后，需要将其部署到测试环境中进行测试。测试环境应该尽可能地模拟生产环境，以确保代码能够正常运行并符合要求。
5. 测试验证：在测试环境中进行完整的测试和验证，确保代码功能正常、性能达标、兼容性良好等。
6. 生产环境部署：当代码在测试环境中通过测试并验证无误后，可以将其部署到生产环境中。在生产环境中，需要注意安全性和可用性问题，并对系统进行监控以确保系统稳定运行。
7. 上线发布：在完成以上步骤后，就可以正式发布上线了。在发布上线前，需要进行最后的确认和验证，确保代码的完整性和正确性。



#### 函数式编程和面向对象

函数式编程和面向对象编程是两种不同的编程范式，它们有各自的优缺点。

函数式编程注重于函数的使用和组合，通过组合一些简单的函数来实现复杂的功能。函数式编程通常使用不可变数据，避免了副作用并且更容易推理和调试。同时，函数式编程也支持高阶函数、柯里化和递归等技术，这些都可以帮助我们编写更加灵活、简洁和高效的代码。

面向对象编程则强调封装、继承和多态。通过将数据和操作打包在一起，面向对象编程使得代码更加模块化，并且能够更好地应对复杂的需求。面向对象编程通常使用可变状态，这样可以更直接地进行数据修改和操作。但是在处理并发和多线程等场景时，可能会出现线程安全问题。



#### 团队的前端开发模式

前端开发模式是影响开发效率和质量的重要因素之一。下面介绍几种优化前端开发模式的方法：

1. 使用自动化构建工具：使用构建工具来自动化打包、压缩、合并和编译代码，从而避免手动重复操作，提高开发效率。常见的构建工具有Webpack、Gulp、Grunt等。
2. 统一开发环境：在团队内部统一开发环境，确保所有开发人员都使用相同的IDE、编辑器和浏览器，可以避免因为环境不同导致的代码运行异常等问题。
3. 使用代码规范和风格指南：通过制定代码规范和风格指南，可以让团队成员保持一致的编码风格，提高代码可读性和维护性。常见的代码规范和风格指南有ESLint、Prettier等。
4. 实现组件化开发：将页面拆分成多个独立的组件，每个组件都有自己的样式、JavaScript和HTML模板，通过组件库实现复用，提高开发效率和代码的可维护性。
5. 定期进行Code Review：定期对代码进行Review，发现潜在的问题和错误，并及时纠正，以保证代码质量和稳定性。
6. 使用性能工具进行优化：使用性能工具诊断和优化代码，如Chrome DevTools、Lighthouse、WebPageTest等，可以帮助开发人员及时发现并解决性能问题，从而提高页面加载速度和用户体验。
7. 遵循敏捷开发原则：采用敏捷开发的原则，例如迭代式、持续交付、快速反馈、团队合作等，可以更快地响应需求变化，提高产品质量和团队协作效率。





#### **项目鉴权 - 复杂组件拆分方案**

1.将权限控制按层级分为模块，菜单，按钮3大层级，因为当整个模块都没有权限的时候，不必再判断按钮权限，提高性能，菜单亦如此。

2.code实际是用英文字符来表示，增强可读性，例如设备菜单是deviceMenu，而不是无意义的id。

3.实际鉴权函数入参为模块，菜单，按钮3个参数，更好理解也更加符合直觉，模块下有多个菜单，菜单下有多个按钮，也就是说几乎每个添加按钮都可以取名“add”，不必担心重复。

4.将列表字段当按钮处理，甚至可以实现同一列表每个字段的权限控制，例如部分敏感价格字段显示为**（当然安全性不高）

5.后端只需返回当前用户角色拥有的全部模块，菜单，按钮code即可



#### **组件拆分原则**

- **一切都是为了可读性和可维护性**
- 组件有两种类型：**一种是有状态的，一种是无状态的**
- **逻辑组件（智能组件）、UI组件（木偶组件）** 、路由组件、状态组件（当前工作环境等常量）
- **业务代码的复用高于代码的复用**



- 不同业务模块之间的数据交互（在交互设计上，需要尽可能做到功能分离化）
- 拆分足够细，尽量使用函数式编程（有很多方向，只是选择了其中的一种）



**写代码的时候，不管碰到了什么问题，如果影响到了可读性，而自己会一时半会没想到怎么解决，或时间来不及的时候。永远要选择可读性。**



寻求低耦合,易维护,低复杂度三者之间的平衡



**最后，做好组件的单一原则，做好接口的输入输出。**





#### **代码review规范：**

目的：1 统一编码风格，规范，提高团队协作效率，输出可读性强，易维护的代码。

怎么做的：



1 建立完善的csslint、eslint等提交规范，在本地提交阶段注重风格统一。

2 通过git规范，划分分支管理和commit规范。

3 规范项目文件组织和命名，组件命名，文档注释规范



紧急情况来不及审查代码怎么办？

当前mr创建一个tag，跟踪后续补上review。



1 先设计再写代码，减少discussion。

2 拆分pr，减轻审查者的压力。 

3 最好的阶段是产品测试通过审查后再提交mr，避免需求变更导致的重复修改。





#### 依赖注入

1.什么是依赖注入？

在实际开发中，我们在执行某个类（如上述的`class UserService`）时，需要依赖其他的`依赖项`（如：`AuthService`）支撑。我们可以通过`依赖注入框架`(即上述的`外部源`)将执行类的`依赖项`进行分析提取，然后实例化，最后再将实例化后的结果自动注入到该类，来完成对`依赖项`的引用。而不是在`执行类`中直接创建它们。这就是**依赖注入**。

2.什么是控制反转？

起初，我们需要在执行某个类时，需要手动的去创建它的所有依赖项。而使用`依赖注入`的方式实现后，我们将创建依赖项的逻辑放在了`依赖注入框架`(简称`DI`)中，由`DI`框架控制其创建逻辑，而不是在业务代码中手动去控制，这就是**控制反转**。一句话来说，就是将“创建依赖项”的控制权，由程序本身，转移到了`DI框架`中。

3.控制反转和依赖注入的关系？

`控制反转`是一种设计原则，`依赖注入`是一种设计模式。系统的最终目的是实现`控制反转`实现解耦，而`依赖注入`是实现`控制反转`的一种手段。

4.为什么需要依赖注入？

一定程度上实现了松耦合，我们不需要担心执行某个类时，其依赖项的修改导致执行类也需要修改。同时，在执行类中，不会去创建关联其他依赖项，这也让单元测试更加容易。



实现：

大概实现思路是分如下几步：

1.执行某个类之前进行拦截

2.提取执行类中的依赖项，其中包括依赖项目的依赖项

3.实例化各个依赖项

4.将实例化后注入到执行类中