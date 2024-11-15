## React 和 Vue对比

####  对 React 和 Vue 的理解，它们的异同

相似之处：

●都将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库；
●都有自己的构建工具，能让你得到一个根据最佳实践设置的项目模板；
●都使用了Virtual DOM（虚拟DOM）提高重绘性能；
●都有props的概念，允许组件间的数据传递；
●都鼓励组件化应用，将应用分拆成一个个功能明确的模块，提高复用性。

不同之处 ：

1）数据流
Vue默认支持数据双向绑定，而React一直提倡单向数据流 
2）虚拟DOM
Vue2.x开始引入"Virtual DOM"，消除了和React在这方面的差异，但是在具体的细节还是有各自的特点。 
●Vue宣称可以更快地计算出Virtual DOM的差异，这是由于它在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。
●对于React而言，每当应用的状态被改变时，全部子组件都会重新渲染。当然，这可以通过 PureComponent/shouldComponentUpdate这个生命周期方法来进行控制，但Vue将此视为默认的优化。
3）组件化
React与Vue最大的不同是模板的编写。
●Vue鼓励写近似常规HTML的模板。写起来很接近标准 HTML元素，只是多了一些属性。
●React推荐你所有的模板通用JavaScript的语法扩展——JSX书写。

具体来讲：React中render函数是支持闭包特性的，所以import的组件在render中可以直接调用。但是在Vue中，由于模板中使用的数据都必须挂在 this 上进行一次中转，所以 import 一个组件完了之后，还需要在 components 中再声明下。
4）监听数据变化的实现原理不同
●Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
●React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的vDOM的重新渲染。这是因为 Vue 使用的是可变数据，而React更强调数据的不可变。
5）高阶组件
react可以通过高阶组件（HOC）来扩展，而Vue需要通过mixins来扩展。



#### **react 和vue router的区别**

React 和 Vue 都提供了路由管理工具，React 的是 React Router，Vue 的是 Vue Router。它们的区别在于：

1. 语法不同：React Router 基于 JSX 语法，而 Vue Router 则是基于 Vue 组件的方式来定义路由。
2. 动态路由的处理方式不同：React Router 将动态参数放在 path 中（例如 /users/:id），而 Vue Router 将动态参数放在路由的 props 中。
3. 路由钩子函数的使用方式不同：React Router 使用类似于 React 组件生命周期函数的方式来使用路由钩子函数，而 Vue Router 则是通过在路由配置对象中定义 beforeEnter、beforeRouteUpdate 等钩子函数来实现。
4. 组件通信方式不同：在 React Router 中，组件之间的通信是通过 props 来实现的，而在 Vue Router 中则可以直接通过 Vuex 状态管理库进行通信。





#### React Hoc/Vue mixin 有什么区别，以及各自的优劣性

React HOC（Higher-Order Component）和 Vue Mixin 都是一种代码重用的技术，它们可以将组件之间的共同逻辑提取出来，并注入到多个不同的组件中。但是它们在实现方式、使用方式和功能上有一些明显的区别。

1. 实现方式

在 React 中，HOC 是一个高阶函数，它接收一个组件作为参数并返回一个新的增强组件。而 Vue Mixin 则是一个普通的 JavaScript 对象，包含数据、方法、生命周期钩子等属性。

1. 使用方式

在 React 中，我们可以使用 HOC 来包装一个组件，然后通过 props 的形式将共同逻辑传递给包装组件和内部组件。在 Vue 中，Mixins 可以被组件引入，在组件选项中通过 mixins 属性进行配置。

1. 功能

React HOC 可以通过组合模式和高阶函数等技术，实现更加灵活和可复用的代码。它可以实现类似于渲染劫持、状态管理、权限控制、错误边界等功能。而 Vue Mixin 也可以提供类似的功能，比如公共方法、computed 属性、生命周期钩子等。

1. 优劣性

React HOC 的优点是能够实现更高度的复用性、更好的拆分和解耦，同时也能够方便地处理状态管理、渲染劫持等方面的问题。但是如果使用过多，会导致组件嵌套层次加深，降低代码可读性。

Vue Mixin 的优点在于可以提供公共方法、数据、生命周期钩子等功能，同时也能够减少代码重复，提高代码的可维护性和可扩展性。但是如果滥用 Mixin，会导致命名冲突和状态管理问题。

总体而言，在 React 中 HOC 更为强大，适合需要更高度抽象的场景；而在 Vue 中 Mixin 的应用更加普遍，适合一些简单场景下的代码复用。开发者应该根据实际情况选择合适的技术方案。



#### react diff算法和vue diff算法的区别

React 的 diff 算法：

React 的 diff 算法是基于比较虚拟 DOM 树的策略。在每次组件更新时，React 会先生成一棵新的虚拟 DOM 树。然后通过对比新旧两棵虚拟 DOM 树的差异，来决定需要进行哪些 DOM 操作。这个过程就叫做 Reconciliation（协调）。

React 的 diff 算法可以说是一种全量算法，因为它需要递归遍历整棵虚拟 DOM 树，并且对于同层级节点的比较也是完全的。具体来说，React 的 diff 算法会按照以下规则进行比较：

1. 若不同类型的元素，则直接替换
2. 若同一类型的元素，则只需要比较其属性和子元素即可，如果有差异则进行更新
3. 若在同一位置上的元素不同，则替换该位置上的元素
4. 若新的元素比旧的多，则添加新的元素
5. 若新的元素比旧的少，则删除多余的元素

Vue 的 diff 算法：

Vue 的 diff 算法采用的是双端比较的方式，即从头和尾同时开始比较节点。这种方式的优势在于可以尽早地发现差异，并且通常情况下能够减少比较次数。Vue 还针对列表渲染做了优化，使用了key的机制来判断同层级节点是否相同，如果key相同，则直接复用旧节点，否则才会重新渲染该节点。

Vue 的 diff 算法大致分为以下几步：

1. 查找新旧节点的最长公共子序列（LCS）
2. 如果 LSC 中的节点不需要移动，则直接更新其属性和子元素
3. 如果 LSC 中的节点需要移动，则进行移动操作
4. 对于剩余的未处理节点，有新增节点和删除节点两种情况
5. 新增节点通过插入操作完成，删除节点通过删除操作完成

总结：

React 和 Vue 的 diff 算法在实现细节上略有不同，但都是非常优秀的算法。React 的 diff 算法采用全量比较的方式，可以保证准确性，但在组件树较大时可能会出现性能问题；而 Vue 的 diff 算法采用双端比较的方式以及 key 机制，可以尽早发现差异并减少比较次数，从而提高性能。因此，在实际项目中，需要根据具体的场景选择合适的框架和算法。


