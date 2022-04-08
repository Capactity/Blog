# JavaScript原型与原型链的理解

> 原型有三个部分组成，`prototype`、`_proto_`、`constructor`。

`prototype`：每个函数都会有的属性，它指向函数的原型对象。（函数对象才有，普通对象没有）。

`_proto_`：每一个对象都会有的属性，包含函数对象，它指向构造函数的原型对象。

`constructor`：原型对象上的一个属性，它指向对象的构造函数。



通过示例验证其中的关系：

````typescript
function Pig(name) {
  this.name = name;
}
var peppa = new Pig('peppa');
````



用`new`关键字实例化一个Pig对象，返回一个普通对象pie示例，在实例化的时候，Pig的prototype上的属性会作为原型对象赋值给实例。也就是说peppa的原型，是从Pig的prototype引用来的，即`peppa.__proto__ === Pig.prototype`;

````typescript
peppa.__proto__ === Pig.prototype;
// true;
````



Pig是一个函数对象，它是Function的一个实例，所以`Pig.__proto__ === Function.prototype`一定为true; 

````typescript
Pig.__proto__ === Function.prototype;
// true
````

