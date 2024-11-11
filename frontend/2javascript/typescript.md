##### **1. 什么是TypeScript？**

> Typescript 是一个强类型的 JavaScript 超集，支持ES6语法，支持面向对象编程的概念，如类、接口、继承、泛型等。Typescript并不直接在浏览器上运行，需要编译器编译成纯Javascript来运行。



##### 在进行项目选型时，我会考虑以下因素来决定是否选择 TypeScript：

1. 项目规模：如果项目规模较大或者预计会增长，那么使用 TypeScript 可以帮助开发人员更好地管理代码，降低维护成本。

2. 团队能力：如果团队中有一定的 TypeScript 经验和技能，那么使用 TypeScript 会提高开发效率和代码质量。

3. 开发周期：如果时间紧迫，使用 TypeScript 可以帮助开发人员更快地编写安全、可读性高的代码。

4. 需求稳定性：如果项目需求相对稳定，而不需要频繁变化，使用 TypeScript 可以帮助减少错误，并提供更好的类型提示和错误检查。

5. 类型安全性：如果需要确保代码的类型安全性，使用 TypeScript 可以在编译期间发现错误，而不是在运行时。

6. 第三方库支持：如果项目需要集成第三方库，而这些库已经提供了 TypeScript 定义文件，那么使用 TypeScript 可以提供更好的集成体验和类型提示。

   

##### **2. 为什么要使用 TypeScript ? TypeScript 相对于 JavaScript 的优势是什么？**

> 增加了静态类型，可以在开发人员编写脚本时检测错误，使得代码质量更好，更健壮。
>  优势: 
>  \1. 杜绝手误导致的变量名写错; 
>  \2. 类型可以一定程度上充当文档; 
>  \3. IDE自动填充，自动联想;

##### **3. TypeScript 中 const 和 readonly 的区别？枚举和常量枚举的区别？接口和类型别名的区别？**

> `const 和 readonly`: const可以防止变量的值被修改，readonly可以防止变量的属性被修改。
>  `枚举和常量枚举`: 常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除。 常量枚举成员在使用的地方会被内联进来。 之所以可以这么做是因为，常量枚举不允许包含计算成员。
>  `接口和类型别名`: 两者都可以用来描述对象或函数的类型。与接口不同，类型别名还可以用于其他类型，如基本类型（原始值）、联合类型、元组。

##### **5. TypeScript 中 any、never、unknown、null & undefined 和 void 有什么区别？**

> `any`: 动态的变量类型（失去了类型检查的作用）。
>  `never`: 永不存在的值的类型。例如：never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。
>  `unknown`: 任何类型的值都可以赋给 unknown 类型，但是 unknown 类型的值只能赋给 unknown 本身和 any 类型。
>  `null & undefined`: 默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和  undefined 赋值给 number 类型的变量。当你指定了 --strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。
>  `void`: 没有任何类型。例如：一个函数如果没有返回值，那么返回值可以定义为void。

##### **8. TypeScript 中的 this 和 JavaScript 中的 this 有什么差异？**

> 1. TypeScript：noImplicitThis: true 的情况下，必须去声明 this 的类型，才能在函数或者对象中使用this。
> 2. Typescript 中箭头函数的 this 和 ES6 中箭头函数中的 this 是一致的。







#### 类型



##### TypeScript 中类型有哪些分类？

答：TypeScript 中的类型可以分为基本类型、对象类型、联合类型、交叉类型、函数类型和枚举类型等。

##### 基本类型：boolean、数字、字符串、数组、元祖（Tuple）、枚举、any、void、null & undefined、object、

元祖类型主要应用于已经明确具体数量及类型的数组，比如二维向量，三位向量等。是由多个类型合并的数组，

枚举类型是一种有限集合的数据类型。可以使用枚举类型定义一些常量或者预定义的值，枚举中默认从0开始计数，你也可以手动的指定成员的数值

enum Color {Red = 1, Green, Blue} let c: Color = Color.Green; // 2



##### Type 和 Interface 有什么区别？

答：Type 和 Interface 都可以用来定义类型，主要的区别在于语法和用途。Type 可以定义基本类型、联合类型、交叉类型和函数类型等，而 Interface 主要用于定义对象类型和类的形状。此外，Interface 还可以被扩展，而 Type 不行。

> \1. 都可以描述 '对象' 或者 '函数' 
> \2. 都允许拓展(extends)
>
> 不同点： 
> \1. type 可以声明基本类型，联合类型，元组
> \2. type 可以使用 typeof 获取实例的类型进行赋值
> \3. 多个相同的 interface 声明可以自动合并
>
> 使用 interface 描述‘数据结构’，使用 type 描述‘类型关系’



##### Partial 和 Required 分别是什么作用？

答：Partial 和 Required 都是 TypeScript 中的内置工具类型。Partial<T> 可以将 T 中所有属性变成可选的，而 Required<T> 可以将 T 中所有属性变成必填的。

例如：

```
typescriptCopy Codeinterface User {
  name: string;
  age?: number;
}

type OptionalUser = Partial<User>; // { name?: string; age?: number }
type RequiredUser = Required<User>; // { name: string; age: number }
```

##### keyof 关键字的作用是什么？

答：keyof 关键字可以获取一个类型中所有属性名组成的联合类型。例如：

```
typescriptCopy Codeinterface User {
  name: string;
  age: number;
}

type UserKeys = keyof User; // 'name' | 'age'
```

##### Pick 和 Omit 分别是什么作用？

答：Pick 和 Omit 都是 TypeScript 中的内置工具类型。Pick<T, K> 可以从 T 中选取部分属性组成一个新的类型，而 Omit<T, K> 可以从 T 中排除指定属性组成一个新的类型。



##### 简述工具类型 `Exclude`、`Omit`、`Merge`、`Intersection`、`Overwrite`的作用。**

> `Exclude<T, U>` 从 `T` 中排除出可分配给 `U`的元素。
> `Omit<T, K>` 的作用是忽略`T`中的某些属性。
> `Merge<O1, O2>` 是将两个对象的属性合并。
> `Compute<A & B>` 是将交叉类型合并
> `Intersection<T, U>`的作用是取`T`的属性,此属性同样也存在与`U`。
> `Overwrite<T, U>` 是用`U`的属性覆盖`T`的相同属性。



##### **类型的全局声明和局部声明**

> 如果声明文件内不包含`import、export`，那么这个文件声明的类型就会变成全局声明。反之，若是这个文件包含了`import、export`，那么这个文件包含的类型声明则会是局部声明，不会影响到全局声明。



##### **20. TypeScript 中同名的 interface 或者同名的 interface 和 class 可以合并吗？**

> 同名的interface会自动合并，同名的interface和class会自动聚合。

##### **21. 如何使 TypeScript 项目引入并识别编译为 JavaScript 的 npm 库包？**

> 1. 选择安装 ts 版本，`npm install @types/包名 --save`；
> 2. 对于没有类型的 js 库，需要编写同名的.d.ts文件

##### **25. 对 TypeScript 类中成员的 public、private、protected、readonly 修饰符的理解？**

> `public`: 成员都默认为`public`，被此限定符修饰的成员是可以被外部访问；
>  `private`: 被此限定符修饰的成员是只可以被类的内部访问；
>  `protected`: 被此限定符修饰的成员是只可以被类的内部以及类的子类访问;
>  `readonly`: 关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

##### **26. keyof 和 typeof 关键字的作用？**

> `keyof 索引类型查询操作符` 获取索引类型的属性名，构成联合类型。
>  `typeof` 获取一个变量或对象的类型。

##### **6. TypeScript 中 interface 可以给 Function / Array / Class（Indexable）做声明吗？**

> ```javascript
> /* 可以 */
> // 函数声明
> interface Say {
>  (name: string): viod;
> }
> let say: Say = (name: string):viod => {}
> // Array 声明
> interface NumberArray { 
>  [index: number]: number; 
> } 
> let fibonacci: NumberArray = [1, 1, 2, 3, 5];
> // Class 声明
> interface PersonalIntl {
>  name: string
>  sayHi (name: string): string
> }
> ```

##### **7. TypeScript 中可以使用 String、Number、Boolean、Symbol、Object 等给类型做声明吗？**

> ```javascript
> /* 可以 */
> let name: string = "bob";
> let decLiteral: number = 6;
> let isDone: boolean = false;
> let sym: symbol = Symbol();
> interface Person {
>  name: string;
>  age: number;
> }
> ```

> 1. 

##### **9. TypeScript 中使用 Union Types 时有哪些注意事项？**

> **属性或方法访问**: 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法。
>
> ```js
> function getLength(something: string | number): number {
>    return something.length;
> }
> // index.ts(2,22): error TS2339: Property 'length' does not exist on type >'string | number'.
> //   Property 'length' does not exist on type 'number'.
> 
> function getString(something: string | number): string {
>    return something.toString();
> }
> // 公共方法和属性可以访问
> 复制代码
> ```

##### **10. TypeScript 如何设计 Class 的声明？**

> ```ts
> class Greeter {
>    greeting: string;
>    constructor(message: string) {
>        this.greeting = message;
>    }
>    greet(): string{
>        return "Hello, " + this.greeting;
>    }
> }
> let greeter = new Greeter("world");
> // 在声明类的时候，一般类中都会包含，构造函数、对构造函数中的属性进行类型声明、类中的方法。
> 复制代码
> ```

##### **11. TypeScript 中如何联合枚举类型的 Key?**

> ```ts
> enum str {
>    A,
>    B,
>    C
> }
> type strUnion =  keyof typeof str; // 'A' | 'B' | 'C'
> 复制代码
> ```

> 

##### **15. 简单介绍一下 TypeScript 模块的加载机制？**

> 假设有一个导入语句 `import { a } from "moduleA"`; 
>  \1. 首先，编译器会尝试定位需要导入的模块文件，通过绝对或者相对的路径查找方式； 
>  \2. 如果上面的解析失败了，没有查找到对应的模块，编译器会尝试定位一个`外部模块声明`（.d.ts）； 
>  \3. 最后，如果编译器还是不能解析这个模块，则会抛出一个错误 `error TS2307: Cannot find module 'moduleA'.`

##### **16. 简单聊聊你对 TypeScript 类型兼容性的理解？**

> `ts 类型兼容：` 当一个类型 Y 可以赋值给另一个类型 X 时， 我们就可以说类型 X 兼容类型 Y。也就是说两者在结构上是一致的，而不一定非得通过 extends 的方式继承而来
>  `接口的兼容性：X = Y `只要目标类型 X 中声明的属性变量在源类型 Y 中都存在就是兼容的（ Y 中的类型可以比 X 中的多，但是不能少）
>  `函数的兼容性：X = Y ` Y 的每个参数必须能在 X 里找到对应类型的参数，参数的名字相同与否无所谓，只看它们的类型（参数可以少但是不能多。与接口的兼容性有区别，原因参考第 17 问）

##### **17. 协变、逆变、双变和抗变的理解？**

> `协变：X = Y `Y 类型可以赋值给 X 类型的情况就叫做协变，也可以说是 X 类型兼容 Y 类型
>
> ```typescript
> interface X { name: string; age: number; } 
> interface Y { name: string; age: number; hobbies: string[] }
> let x: X = { name: 'xiaoming', age: 16 }
> let y: Y = { name: 'xiaohong', age: 18, hobbies: ['eat'] }
> x = y
> ```
>
> `逆变：printY = printX` 函数X 类型可以赋值给函数Y 类型，因为函数Y 在调用的时候参数是按照Y类型进行约束的，但是用到的是函数X的X的属性和方法，ts检查结果是类型安全的。这种特性就叫做逆变，函数的参数有逆变的性质。
>
> ```typescript
> let printY: (y: Y) => void
> printY = (y) => { console.log(y.hobbies) }
> let printX: (x: X) => void
> printX = (x) => { console.log(x.name) }
> printY = printX
> ```
>
> `双变（双向协变）：X = Y；Y = X`父类型可以赋值给子类型，子类型可以赋值给父类型，既逆变又协变，叫做“双向协变”（ts2.x 之前支持这种赋值，之后 ts 加了一个编译选项 strictFunctionTypes，设置为 true 就只支持函数参数的逆变，设置为 false 则支持双向协变）
>  `抗变（不变）：`非父子类型之间不会发生型变，只要类型不一样就会报错



> 1. 

##### **22. TypeScript 的 tsconfig.json 中有哪些配置项信息？**

> ```json
> {
>   "files": [],
>   "include": [],
>   "exclude": [],
>   "compileOnSave": false,
>   "extends": "",
>   "compilerOptions": { ... }
> }
> ```
>
> `files` 是一个数组列表，里面包含指定文件的相对或绝对路径，用来指定待编译文件，编译器在编译的时候只会编译包含在files中列出的文件。
>  `include & exclude` 指定编译某些文件，或者指定排除某些文件。
>  `compileOnSave：true` 让IDE在保存文件的时候根据tsconfig.json重新生成文件。
>  `extends` 可以通过指定一个其他的tsconfig.json文件路径，来继承这个配置文件里的配置。
>  `compilerOptions` 编译配置项，如何对具体的ts文件进行编译

##### **23. TypeScript 中如何设置模块导入的路径别名？**

> 通过 tsconfig.json 中的 paths 项来配置:
>
> ```json
> { 
>   "compilerOptions": 
>     {
>       "baseUrl": ".", 
>       "paths": { 
>          "@helper/*": ["src/helper/*"], 
>          "@utils/*": ["src/utils/*"], 
>          ... 
>       } 
>    } 
> }
> 复制代码
> ```

##### **24. declare，declare global是什么？**

> `declare` 是用来定义全局变量、全局函数、全局命名空间、js modules、class等
>  `declare global` 为全局对象 `window` 增加新的属性
>
> ```ts
> declare global { 
>    interface Window { 
>         csrf: string; 
>    }
> }
> 复制代码
> ```

> 

##### 

###### **28. 数组定义的两种方式**

> ```1.html&cssa.html5的新特性●语义化标签●增加型表单●媒体元素●Canvas●地理定位●拖放api●Webstorage●Webworker      参考链接:https://blog.csdn.net/sinat_41696687/article/details/123159705b.语义化标签的理解1)去掉或者丢失样式的时候能够让页面呈现出清晰的结构2)有利于SEO:和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息:爬虫依赖于标签来确定上 下文和各个关键字的权重;3)方便其他设备解析(如屏幕阅读器、盲人阅读器、移动设备)以意义的方式来渲染网页;4)便于团队开发和维护，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵 循这个标准，可以减少差异化。c.对盒子模型的理解d.sessionStorage 、localStorage，cookie的使用、作用域、生命周期 存储的时效，大小，http的交互，同源策略：cookie 可以设置path和domain        时效性：cookie在过期时间之前都有些小，session为一个会话存储在浏览器关闭前生效，localStorage是永久有效       大小：cookie有路劲的概念的，在某个路劲下存储的大小只有4K左右，localStorage和sessionStorage存储大小都是5MB，因浏览器的不同有差异      http交互：cookie会始终在同源的http请求中携带，即cookie会在浏览器和服务器之间来回传递没其他两个没有这个e.全屏左固定，右自适应的案列实现方案考察，顺带flex属性(开放)f.如何重置ant  design的默认样式ts
> type Foo= Array<string>;
> interface Bar { 
>      baz: Array<{ name: string, age: number}>
> }
> 
> type Foo = string[];
> interface Bar { 
>      baz : { name: string, age: number }[] 
> }
> ```