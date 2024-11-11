## Nodejs适用于哪些场景？

1. 后端开发，Nodejs的异步I/O天生适合做Web高并发。
2. BFF开发，比如SSR中间层或者GraphQL中间层。
3. 前端基建，Webpack、Gulp、Babel、Jest等等前端工程化的工具或插件。



## Nodejs的事件循环和浏览器有什么区别？

Node.js 的事件循环和浏览器中的事件循环的区别在于，浏览器的异步任务分为宏任务队列和微任务队列，而Nodejs的异步任务分成了6个任务队列，按执行顺序分别为：

1. timers阶段：处理setTimeout()和setInterval()等定时器事件。
2. I/O callbacks阶段：处理几乎所有的异步I/O回调，例如网络I/O、文件I/O等。
3. idle, prepare阶段：这是Node.js内部使用的，开发者很少会用到。
4. poll阶段：等待新的I/O事件，处理已经完成的事件回调。
5. check阶段：处理setImmediate()的回调函数。
6. close callbacks阶段：处理一些关闭事件，例如socket关闭等。

分析一下整个代码在事件循环的六个阶段中的执行顺序：

1. timers阶段：在该阶段中，执行了由setTimeout方法产生的回调函数，输出`timeout`，并在回调函数中注册了一个process.nextTick方法产生的回调函数。
2. I/O callbacks阶段：执行fs.readFile方法的回调函数，输出`readFile`，并在回调函数中注册了一个setImmediate和一个setTimeout方法产生的回调函数。
3. idle, prepare阶段：没有任务执行。
4. poll阶段：处理setImmediate方法产生的回调函数，输出`immediate`和`immediate in readFile callback`。然后处理由fs.readFile方法产生的setTimeout方法回调函数，输出`timeout in readFile callback`。
5. check阶段：在该阶段中，执行由process.nextTick方法产生的回调函数，输出`nextTick`和`nextTick`。
6. close callbacks阶段：没有任务执行。



## 讲一下EventEmitter？

EventEmitter经常在面试的时候会要求手写，因为这玩意用途实在是太广了。比如在Vue里面的EventBus实现组件通信，其核心就是EventEmitter。

Node.js 的大多数核心模块都是基于EventEmitter实现的，如 http、net、fs，很多第三方库也是基于EventEmitter实现的，如socket.io、nodemailer、cheerio等。

使用EventEmitter的好处是可以用事件的形式来处理异步任务，可以大大简化代码，并且容易处理异常。

举个例子来看看为什么Nodejs里大多数模块都要继承EventEmitter。

这是不使用EventEmitter实现的文件读取，所有逻辑都放在一个回调函数里：

```javascript
const fs = require('fs');

fs.readFile('file.txt', (err, data) => {
  if (err) {
    console.error(`Failed to read file: ${err}`);
  } else {
    console.log(`File content: ${data}`);
  }
});
```

这是使用EventEmitter的文件读取：

```javascript
javascript
复制代码const fs = require('fs');

const stream = fs.createReadStream('file.txt');

stream.on('data', (chunk) => {
  console.log(`Received ${chunk.length} bytes of data.`);
});

stream.on('end', () => {
  console.log('Finished reading file.');
});
```

很显然，使用EventEmitter之后，处理文件和处理异常的逻辑就被分开了，代码可读性和可维护性都提升了。

## Buffer怎么理解，有什么应用？

Buffer对象是一个类似于数组的对象，它的每个元素都是一个表示 8 位字节的整数。

可以将其看作是一个字节数组，用来存储和操作二进制数据。

应用场景：

1. 网络通信：可以使用Buffer.from()方法将字符串转换为二进制数据，然后使用net模块进行网络通信：

```javascript
javascript
复制代码const net = require('net');

const client = net.createConnection({ port: 8080 }, () => {
  // 将字符串转换为二进制数据
  const data = Buffer.from('Hello, world!', 'utf8');

  // 发送数据
  client.write(data);
});
```

1. 文件操作，用Buffer来存储文件数据：

```javascript
javascript
复制代码const fs = require('fs');

// 读取文件，并将数据存储到 Buffer 对象中
const data = fs.readFileSync('/path/to/file');

// 处理数据
// ...
```

1. 加密解密，例如，可以使用 crypto 模块创建加密解密算法需要的二进制数据：

```javascript
javascript
复制代码const crypto = require('crypto');

// 创建加密解密算法需要的二进制数据
const key = Buffer.from('mysecretkey', 'utf8');
const iv = Buffer.alloc(16);

// 创建加密解密算法对象
const cipher = crypto.createCipheriv('aes-256-cbc', key, iv);

// 加密数据
const encrypted = Buffer.concat([cipher.update(data), cipher.final()]);
```

1. 图像处理：

```javascript
javascript
复制代码const fs = require('fs');
const sharp = require('sharp');

// 读取图片文件，并将数据存储到 Buffer 对象中
const data = fs.readFileSync('/path/to/image');

// 处理图片
sharp(data)
  .resize(200, 200)
  .toFile('/path/to/resized-image', (err, info) => {
    // ...
  });
```



## 什么是I/O？

**概念**：计算机里所谓的I/O指的是输入和输出，但对于前端同学而言，这个定义可能不太好理解。简单点说，需要等待的任务都可以称为I/O任务，比如前端的事件处理、网络请求、定时器，后端的文件处理、网络请求、数据库操作，这些都属于I/O任务。

**异步I/O**：以网络请求任务为例，传统的同步I/O指的是一个一个排队执行，一个执行完了再执行下一个，即使线程空闲，也不能执行其他任务。

而异步I/O会返回一个标记，告诉调用者 I/O 操作已经开始，但不会阻塞线程。当 I/O 操作完成时，会调用注册的回调函数，将结果返回给调用者。

异步I/O使得程序在执行 I/O 操作时不必等待，提高了程序的并发性能。

**听起来异步I/O很好，那为什么同步I/O依然会存在**

1. 传统的同步I/O操作比异步I/O操作更容易理解和编写。异步编程需要开发人员具备较高的技能水平，以及对事件循环、回调函数等概念的深入理解。
2. 对于某些小规模的应用程序或者一些低频次的I/O操作，使用异步I/O可能不会带来很大的性能提升，而且可能会增加代码的复杂性。
3. 对于一些CPU密集型任务，传统I/O操作可能比异步I/O更快，因为异步I/O 会在I/O操作执行期间增加额外的上下文切换和事件处理负担，从而降低了程序的性能。

**CPU密集和I/O密集：**

1. CPU密集任务指的是纯计算任务。
2. I/O密集的任务指的是需要等待的任务。所谓的高并发，显然属于I/O密集型任务。



## 什么是Stream流，有哪些应用场景？

Stream是一种处理流式数据的抽象接口，用于读取、写入、转换和操作数据流。它是一个基于事件的 API，可以让我们以高效、低延迟的方式处理大型数据集。

说直白点就是基于Stream封装的API，性能更好。

比如读取文件，使用流我们可以一点一点来读取文件，每次只读取或写入文件的一小部分数据块，而不是一次性将整个文件读取或写入到内存中或磁盘中，这样做能够降低内存占用。

## 9. 什么是BFF？

BFF（Backend For Frontend）,说白了就是中间层，由前端同学开发的后端项目。

最常见的BFF项目像SSR和GraphQL。SSR用来解决SEO问题，GraphQL用来聚合数据，解决API查询的问题。

## 10. 什么是ORM？Nodejs的ORM框架有哪些？

ORM框架是通过对SQL语句进行封装，并将数据库的数据表和用户代码里的模型对象进行自动映射。

这样开发者使用时只需要调用模型对象的方法就能实现对数据库的增删改查，不用手写太多的SQL了。

Node.js中比较流行的ORM框架有：

1. Sequelize：基于JS，在Koa、Express、Egg这样的框架里操作数据库用Sequelize比较多，当然Sequelize经过一定扩展也可以支持TS。
2. TypeORM：基于TS，Nest框架首选TypeORM。



## 讲一下常见的Nodejs框架？

- Koa：一个轻量的Nodejs框架，代码非常简洁。采用洋葱圈模型中间件，非常方便扩展功能，但是开发后端API需要进行再封装。
- Express：Express也是一个轻量框架，Express和Koa的区别在于中间件机制。但总体差别不是很大，绝大多数Nodejs框架都是在Koa或者Express基础上封装的。
- Eggjs：基于Koa封装的框架，整合了数据库、路由、安全防护、日志记录、异常处理等中间件，可以用来快速开发Rest或者Restful API项目。
- Nestjs：基于TS,使用了大量的装饰器语法，开发体验类似于Java的Springboot。除此之外，Nestjs还提供了GraphQL、WebSocket、各种MQ和微服务的解决方案，比较适合大型后端项目的开发。



## 有没有做过数据库优化？

常见的优化有：

1. 使用explain执行计划查看SQL的执行信息，进而定位慢SQL来源。
2. 索引是Mysql调优首先能想到的方案，合理设置索引可以很大程度上提高查询效率。
3. 大分页也是一个常见的性能问题出现的地方，因为MySQL需要扫描大量的数据，造成性能瓶颈。可以通过使用主键或者游标分页的方式来优化。
4. 读写分离，单机顶不住的时候，可以使用主从架构，把数据库读写分担到不同的机器上。
5. 分库分表，如果数据表存了海量数据，除了读写分离之外，还要考虑分库分表，把一张表分成多张表，减轻数据库压力。



## 有了解过分布式和微服务吗？

当单体应用撑不住的时候，就得考虑上集群，把应用部署在多个机器上，就形成了分布式架构。

分布式的集群不仅带来了算力和并发能力，也带来了各种问题，这其中包括：分布式通信、分布式事务、分布式id、分布式容错、负载均衡等。

所以就需要有各种中间件来解决这些问题，比如Nginx、Zookeeper、Dubbo、MQ、RPC等。

然后当项目规模进一步扩大的时候，不仅要考虑集群，还要考虑项目的拆分，这时候就要上微服务架构了。把一个大项目根据业务拆分成很多功能单一的模块，可以由不同的团队独立开发和部署。

比如一个电商的后台API，可以拆分成用户服务、商品服务、订单服务、优惠券服务、广告服务，这些服务由不同的团队去维护。

当然，微服务也带来了更多的复杂性，所以就会有像Spring Cloud、Spring Cloud Alibaba这样的解决方案去解决这些复杂性。