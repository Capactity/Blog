# JavaScript原型与原型链的理解

> 原型有三个部分组成，`prototype`、`_proto_`、`constructor`。

`prototype`：每个函数都会有的属性，它指向函数的原型对象。（函数对象才有，普通对象没有）。

`_proto_`：每一个对象都会有的属性，包含函数对象，它指向构造函数的原型对象。

`constructor`：原型对象上的一个属性，它指向对象的构造函数。

通过示例验证其中的关系：

````typescript
function Person(name) {
  this.name = name;
}
var person = new Person('person');
````

<img src="../img/构造函数.png" alt="构造函数"  />

用`new`关键字实例化一个Person对象，返回一个普通对象person示例，在实例化的时候，Person的prototype上的属性会作为原型对象赋值给实例。也就是说person的原型，是从Person的prototype引用来的，即`person.__proto__ === Person.prototype`;

````typescript
person.__proto__ === Person.prototype;
// true;
````



Person是一个函数对象，它是Function的一个实例，所以`Person.__proto__ === Function.prototype`一定为true; 

````typescript
Person.__proto__ === Function.prototype;
// true
````



上面说到的constructor，它指向原型对象构造函数属性，也就是`person.__proto__.constructor == Person`;

````typescript
person.__proto__.constructor == Person;
// true
````



当然我们有`person.__proto__ === Person.prototype`，所以`Person.prototype.constructor == Person`也为true;

````typescript
Person.prototype.constructor == Person;
// true
````



下面我们来说一下Person、Function、Object之间的关系；
首先，Person为Function的实例，所以有`Person.__proto__ === Function.prototype`为true；

````typescript
Person.__proto__ === Function.prototype;
// true
````



<img src="../img/构造函数与实例.png" alt="构造函数与实例"  />



然后由于构造函数创建的时候会给其函数加上prototype属性，方面后面实例的引用，prototype属于普通对象，为Object的实例，有：

```typescript
Person.prototype.__proto__ === Object.prototype;
// true
```



由上可以知道，所有构造函数的原型对象的`__proto__`都指向Object.prototype

````typescript
Function.prototype.__proto__ === Object.prototype
// true
````



另外，Object为一个对象，可以认为为某一个Function的实例返回，所以有：

````typescript
Object.__proto__ === Function.prototype
// true
````



故得原型链条

`person.__proto__ === Person.prototype` => `Person.__proto__ === Function.prototype` => `Function.prototype.__proto__ === Object.prototype`;

Object似乎已经到了原型链的顶端。

````typescript
Object.prototype.__proto__ === null;
// true
````



<img src="../img/原型链.png" alt="原型链"  />



至此，原型链的概念我们也有了大致的了解，正如图上蓝线标识的。

JS在创建对象的时候，会在新对象上产生一个__proto__的属性，这个属性指向了它构造函数的原型的prototype。由此一级一级向上直到到达Object.prototype.proto === null的这个链条我们称之为原型链。

