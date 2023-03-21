# JavaScript常用的设计模式

> **定义：** 设计模式，即解决某个特定场景下对某种问题的解决方案，当我们遇到合适的场景时，会自然而然想到符合这种场景的设计模式。





## 策略模式

**概念：** 定义一系列的算法，把它们一个个封装起来，并且使它们可以互相转换。



**意义：**

- 利用组合，委托等思想，能有效的避免很多if条件语句。
- 提供了开闭原则，时代码更容易理解和扩展。
- 代码复用。



**应用场景**：当你负责的模块，基本满足以下情况时。

- 各判断条件下的策略相互独立且可复用
- 策略内部逻辑相对复杂
- 策略需要灵活组合

例如：表单校验。

示例：

````typescript
var strategies = {
  isNonEmpty: function(value, errorMsg){
    if (value === '') {
      return errorMsg;
    }
  },
  minLength: function(value, length, errorMsg){
    if (value.length < length) {
      return errorMsg;
    }
  },
  isMobile: function(value, errorMsg){ // 手机号码格式
    if (!/(^1[3|5|8][0-9]{9}$)/.test(value)){
      return errorMsg;
    }
  }
};

var Validator = function(){
  this.cache = []; // 保存校验规则
};
Validator.prototype.add = function(dom, rule, errorMsg) {
  var ary = rule.split(':'); // 把 strategy 和参数分开
  this.cache.push(function (){ // 把校验的步骤用空函数包装起来，并且放入 cache
    var strategy = ary.shift(); // 用户挑选的 strategy
    ary.unshift(dom.value); // 把 input 的 value 添加进参数列表
    ary.push(errorMsg); // 把 errorMsg 添加进参数列表
    return strategies[strategy].apply(dom, ary);
  })
}
Validator.prototype.start = function(){
  for (var i = 0, validatorFunc; validatorFunc = this.cache[i++];){
    var msg = validatorFunc(); // 开始校验，并取得校验后的返回信息
    if (msg){ // 如果有确切的返回值，说明校验没有通过
      return msg;
    }
  }
};

var validataFunc = function(){
  var validator = new Validator(); // 创建一个 validator 对象
    /***************添加一些校验规则****************/
    validator.add(registerForm.userName, 'isNonEmpty', '用户名不能为空');
    validator.add(registerForm.password, 'minLength:6', '密码长度不能少于 6 位');
    validator.add(registerForm.phoneNumber, 'isMobile', '手机号码格式不正确');
    var errorMsg = validator.start(); // 获得校验结果
    return errorMsg; // 返回校验结果
  }
  var registerForm = document.getElementById('registerForm');
  registerForm.onsubmit = function(){
    var errorMsg = validataFunc(); // 如果 errorMsg 有确切的返回值，说明未通过校验 if ( errorMsg ){
    console.log(errorMsg);
    return false; // 阻止表单提交
  }
};
````





## 观察者模式（发布-订阅模式 ）

**概念：** 一种消息范式，定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。



**意义：**

- 支持简单的广播通信，自动通知所有已订阅过的对象。



**观察者模式 VS 发布-订阅模式**

1 观察者模式：

- 耦合度高
- 只有两个主体，目标对象`Subject`，观察者`Observer`。
- 观察者`Observer`需要实现`update`方法，供目标对象调用。

2 发布-订阅模式

- 松散耦合，灵活度高。
- 包含发布者`Publisher`，时间调度中心，订阅者`Subscriber`，相互之间解耦。
- 事件类型过多时，需要注意事件命名规范。



**应用场景**：当你负责的模块，基本满足以下情况时。

- 各模块互相独立
- 存在一对多的依赖关系
- 依赖模块不稳定，依赖关系不稳定
- 各模块由不同人员、团队开发



例如：双向数据绑定。

````typescript
class EventEmitter {
  constructor() {
    this._events = {}
  }
  emit(type) {
    var funL = this._events[type]
    var args = Array.from(arguments).slice(1)
    funL.forEach(function(e) {
      e(...args)
    })
  }
  addListener(type, func) {
    this._events = this._events || {}
    if (!this._events[type]) {
      this._events[type] = []
    }
    this._events[type].push(func)
  }
  removeListener(type, func) {
    this._events[type].splice(this._events[type].indexOf(func), 1)
  }
  on(type, func) {
    this.addListener(type, func)
  }
}
//  首先实例化 EventEmitter 然后添加监听事件以及触发事件。
var emitter = new EventEmitter()
emitter.addListener('test', function(a1, a2) {
  console.log(a1 + a2)
})
emitter.on('test', function(a1, a2) {
  console.log(a2)
})
emitter.emit('test', 'augument1', 'argument2')
emitter.emit('test', 'augument3', 'argument4')
````





## 单例模式

**概念：** 一个类仅有一个实例，并提供一个访问它的全局访问方式。实现时先判断是否有实现的实例，如果没有则创建一个实例再返回，确保一个类只有一个实例。



**意义：**

- 对象只有一个实例，节约资源及性能
- 提供对唯一实例的访问方式
- 可以灵活改变实例化过程



**应用场景**：redux下的store，浏览器添加浮框。

示例：

- 各模块互相独立

````typescript
const Singleton = name => {
  this.name = name
}
Singleton.prototype.getName = () => {
  console.log(this.name)
}

Singleton.getInstance = (() => {
  const instance = null
  return name => {
    if (!instance) {
      // 如果没有当前实例，则创建实例
      instance = new Singleton(name)
    }
    return instance
  }
})()

const a = Singleton.getInstance('sven1')
const b = Singleton.getInstance('sven2')
console.log(a === b) // true
````



## 代理模式

**概念：** 为使用对象提供一个代用方法，为使用者提供控制访问。

`代理模式就是我们不直接访问原对象，通过访问代理对象访问原对象。比如你是一个导演，你肯定不会直接联系明星，而是联系明星的经纪人，由明星的经纪人来决定明星是否和你合作。`



**意义：**

- 单一职责，只有代理对象才能引起该对象的变化
- 用代理过滤一些不合规的访问，减少对对象的访问，一定程度上可以提升性能



**应用场景**：图片预加载，mobx的变量拦截。

示例：

````typescript
var myImage = (function() {
  var imgNode = document.createElement('img')
  document.body.appendChild(imgNode)
  return {
    setSrc: function(src) {
      imgNode.src = src
    }
  }
})()
var proxyImage = (function() {
  var img = new Image()
  img.onload = function() {
    myImage.setSrc(this.src)
  }
  return {
    setSrc: function(src) {
      myImage.setSrc('loading.gif')
      img.src = src
    }
  }
})()

proxyImage.setSrc('http://imgcache.qq.com/music/photo/k/000GGDys0yA0Nk.jpg')
````



## 装饰器模式

**概念：**  在不改变该对象的基础上，在使用期间动态的该对象添加方法。

`例如，你很柔弱，无法保护你女朋友，你通过健身，拥有了一身肌肉。`



**意义：**

- 拓展了对象方法
- 原有方法维持不变，通过挂载各种方法丰富对象使用性。函数方法解耦，拆分成可复用的函数，再挂载到其他函数方法上，实现了复用性。



**应用场景**：动态表单验证。

示例：

````typescript
function Sale(price) {
  this.price = price
  this.decorateList = []
}
Sale.decorators = {}
Sale.decorators.quebec = {
  getPrice: function(price) {
    var price = this.uber.getPrice()
    return price * 0.7 //对price进行处理
  }
}
Sale.decorators.money = {
  getPrice: function(price) {
    var price = this.uber.getPrice()
    return '$' + price * 0.9 //对price进行处理
  }
}
Sale.prototype.decorate = function(decorator) {
  this.decorateList.push(decorator)
}
Sale.prototype.getPrice = function() {
  var price = this.price
  this.decorateList.forEach(function(name) {
    price = Sale.decorators[name].getPrice(price)
  })
  return price
}
var sale = new Sale(100)
sale = sale.decorate('quebec') //魁北克省税
sale = sale.decorate('money') //转为美元格式
console.log(sale.getPrice()) //$50.4
````



## 工厂模式

**概念：**它提供了一种创建对象的最佳方式。在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。实现了创建者和调用者分离，工厂模式分为简单工厂、工厂方法、抽象工厂模式

`例如：通过传入不同短参数达到实例化不同的对象这一目的。比如: 有一天你去餐厅想吃宫保鸡丁，然后餐厅给你实例化了宫保鸡丁。`

**意义：**

- 降低耦合，统一管理和控制，和实现类解耦。



## 抽象工厂模式

**概念：**工厂模式里有约束当前实例的抽象类且有多个（1个以上的）

`比如宫保鸡丁和西红柿炒鸡蛋都可以吃那么他们有共同的结构吃，又比如饭店有做菜的功能，其他饭店也可以做菜，那么做菜这个方法可以作为抽象类来约束实例`

意义：

- 违反了开闭原则，添加系统复杂性
- 我们不需要具体细节，只要看当前抽象类的方法（结构）进行编程就可以了



## 适配器模式

**概念：**我们最常见的适配器模式就是当一个老接口和新接口同时使用，老接口可以 使用适配器去调用。这样我们的代码就更加容易的解耦。

比如你去欧洲工作，他们的插线板你是不能直接使用的，需要在咱们的充电器外面加一个适配器然后就可以使用他们的插线板了



## 设计模式的六大原则

- 开闭原则：对扩展开放，对修改封闭
- 接口隔离原则：降低类之间的耦合度，使用多个隔离的接口
- 单一职责：一个方法只负责一件事情
- 迪米特法则：高内聚，低耦合。

