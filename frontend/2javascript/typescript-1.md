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

##### **5. TypeScript 中 any、never、unknown、null & undefined 和 void 有什么区别？**

> `any`: 动态的变量类型（失去了类型检查的作用）。
>  `never`: 永不存在的值的类型。例如：never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。
>  `unknown`: 任何类型的值都可以赋给 unknown 类型，但是 unknown 类型的值只能赋给 unknown 本身和 any 类型。
>  `null & undefined`: 默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和  undefined 赋值给 number 类型的变量。当你指定了 --strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。
>  `void`: 没有任何类型。例如：一个函数如果没有返回值，那么返回值可以定义为void。

#### 类型

##### TypeScript 中类型有哪些分类？

答：TypeScript 中的类型可以分为基本类型、对象类型、联合类型、交叉类型、函数类型和枚举类型等。

##### 基本类型：boolean、数字、字符串、数组、元祖（Tuple）、枚举、any、void、null & undefined、object、

元祖类型主要应用于已经明确具体数量及类型的数组，比如二维向量，三位向量等。是由多个类型合并的数组，

枚举类型是一种有限集合的数据类型。可以使用枚举类型定义一些常量或者预定义的值，枚举中默认从0开始计数，你也可以手动的指定成员的数值

enum Color {Red = 1, Green, Blue} let c: Color = Color.Green; // 2



##### Pick 和 Omit 分别是什么作用？

答：Pick 和 Omit 都是 TypeScript 中的内置工具类型。Pick<T, K> 可以从 T 中选取部分属性组成一个新的类型，而 Omit<T, K> 可以从 T 中排除指定属性组成一个新的类型。



##### 简述工具类型 `Exclude`、`Omit`、`Merge`、`Intersection`、`Overwrite`的作用。**

> `Exclude<T, U>` 从 `T` 中排除出可分配给 `U`的元素。
> `Omit<T, K>` 的作用是忽略`T`中的某些属性。
> `Merge<O1, O2>` 是将两个对象的属性合并。
> `Compute<A & B>` 是将交叉类型合并
> `Intersection<T, U>`的作用是取`T`的属性,此属性同样也存在与`U`。
> `Overwrite<T, U>` 是用`U`的属性覆盖`T`的相同属性。

##### **21. 如何使 TypeScript 项目引入并识别编译为 JavaScript 的 npm 库包？**

> 1. 选择安装 ts 版本，`npm install @types/包名 --save`；
> 2. 对于没有类型的 js 库，需要编写同名的.d.ts文件

##### **15. 简单介绍一下 TypeScript 模块的加载机制？**

> 假设有一个导入语句 `import { a } from "moduleA"`; 
>  \1. 首先，编译器会尝试定位需要导入的模块文件，通过绝对或者相对的路径查找方式； 
>  \2. 如果上面的解析失败了，没有查找到对应的模块，编译器会尝试定位一个`外部模块声明`（.d.ts）； 
>  \3. 最后，如果编译器还是不能解析这个模块，则会抛出一个错误 `error TS2307: Cannot find module 'moduleA'.`

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

