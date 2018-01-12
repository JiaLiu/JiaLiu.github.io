# 在 Node.js 中使用 async 函数的最佳实践

> 本文译自 [Node.js Async Function Best Practices](https://nemethgergely.com/async-function-best-practices/)，作者是 Gergely Nemeth。如果你对他感兴趣，可以关注他的 [Twitter](https://twitter.com/nthgergo)，以及[订阅](https://www.getrevue.co/profile/gergelyke)他的邮件列表。

借助于新版 V8 引擎，Node.js 从 7.6 开始支持 async 函数特性。今年 10 月 31 日，[Node.js 8 也开始成为新的长期支持版本](https://github.com/nodejs/Release)，因此你完全可以放心大胆地在你的代码中使用 async 函数了。在这边文章里，我会简要地介绍一下什么是 async 函数，以及它会如何改变我们编写 Node.js 应用的方式。

## 什么是 ```async``` 函数

利用 ```async``` 函数，你可以把基于 ```Promise``` 的异步代码写得就像同步代码一样。一旦你使用 ```async``` 关键字来定义了一个函数，那你就可以在这个函数内使用 ```await``` 关键字。当一个 ```async``` 函数被调用时，它会返回一个 ```Promise```。当这个 ```async``` 函数返回一个值时，那个 ```Promise``` 就会被实现；而如果函数中抛出一个错误，那么 ```Promise``` 就会被拒绝。

```await``` 关键字可以被用来等待一个 ```Promise``` 被解决并返回其实现的值。如果传给 ```await``` 的值不是一个 ```Promise```，那它会把这个值转化为一个已解决的 ```Promise```。

```javascript
const rp = require('request-promise')
async function main () {
  const result = await rp('https://google.com')
  const twenty = await 20
  
  // 睡个1秒钟
  await new Promise (resolve => {
    setTimeout(resolve, 1000)
  })
  return result
}
main()
  .then(console.log)
  .catch(console.error)
```

## 向 ```async``` 函数迁移

如果你的 Node.js 应用已经在使用```Promise```，那你只需要把原先的链式调用改写为对你的这些 ```Promise``` 进行 ```await```。

如果你的应用还在使用回调函数，那你应该以渐进的方式转向使用 ```async``` 函数。你可以在开发一些新功能的时候使用这项新技术。当你必须调用一些旧有的代码时，你可以简单地把它们包裹成为 Promise 再用新的方式调用。

要做到这一点，你可以使用内建的 ```util.promisify```方法：

```javascript
const util = require('util')
const {readFile} = require('fs')
const readFileAsync = util.promisify(readFile)
async function main () {
  const result = await readFileAsync('.gitignore')
  return result
}
main()
  .then(console.log)
  .catch(console.error)
```

## ```Async``` 函数的最佳实践

### 在 ```express``` 中使用 ```async``` 函数

```express``` 本来就支持 Promise，所以在 ```express``` 中使用 ```async``` 函数是比较简单的：

```javascript
const express = require('express')
const app = express()
app.get('/', async (request, response) => {
  // 在这里等待 Promise
  // 如果你只是在等待一个单独的 Promise，你其实可以直接将将它作为返回值返回，不需要使用 await 去等待。
  const result = await getContent()
  response.send(result)
})
app.listen(process.env.PORT)
```

但正如 Keith Smith 所指出的，上面这个例子有一个严重的问题——如果 Promise 最终被拒绝，由于这里没有进行错误处理，那这个 ```express``` 路由处理器就会被挂起。

为了修正这个问题，你应该把你的异步处理器包裹在一个对错误进行处理的函数中：

```javascript
const awaitHandlerFactory = (middleware) => {
  return async (req, res, next) => {
    try {
      await middleware(req, res, next)
    } catch (err) {
      next(err)
    }
  }
}
// 然后这样使用：
app.get('/', awaitHandlerFactory(async (request, response) => {
  const result = await getContent()
  response.send(result)
}))
```

### 并行执行

比如说你正在编写这样一个程序，一个操作需要两个输入，其中一个来自于数据库，另一个则来自于一个外部服务：

```javascript
async function main () {
  const user = await Users.fetch(userId)
  const product = await Products.fetch(productId)
  await makePurchase(user, product)
}
```

在这个例子中，会发生什么呢？

* 你的代码会首先去获取 ```user```，
* 然后获取 ```product```，
* 最后再进行支付。

如你所见，由于前两步之间并没有相互依赖关系，其实你完全可以将它们并行执行。这里，你应该使用 ```Promise.all``` 方法：

```javascript
async function main () {
  const [user, product] = await Promise.all([
    Users.fetch(userId),
    Products.fetch(productId)
  ])
  await makePurchase(user, product)
}
```

而有时候，你只需要其中最快被解决的 Promise 的返回值——这时，你可以使用 ```Promise.race``` 方法。

### 错误处理

考虑下面这个例子：

```javascript
async function main () {
  await new Promise((resolve, reject) => {
    reject(new Error('💥'))
  })
}
main()
  .then(console.log)
```

当执行这段代码的时候，你会看到类似这样的信息：

```
(node:69738) UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 2): Error: 💥
(node:69738) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

在较新的 Node.js 版本中，如果 Promise 被拒绝且未得到处理，整个 Node.js 进程就会被中断。因此必要的时候你应该使用 ```try-catch```：

```javascript
const util = require('util')
async function main () {
  try {
    await new Promise((resolve, reject) => {
      reject(new Error('💥'))
    })
  } catch (err) {
    // 在这里处理错误
    // 根据你的需要，有时候把错误直接再抛出也是可行的
  }
}
main()
  .then(console.log)
  .catch(console.error)
```

可是，使用 ```try-catch``` 可能会隐藏掉一些重要的异常，比如像系统错误，你可能更想把它再抛出来。关于在什么情况下你应该将错误再次抛出，我强烈建议你去读一下 Eran 的[这篇文章](https://medium.com/@eranhammer/learning-to-throw-again-79b498504d28)。

### 更为复杂的流程控制

[Caolan McMahon](https://twitter.com/caolan) 的 [async](http://caolan.github.io/async/) 是一个出现较早的用于 Node.js 中异步流程控制的库。它提供了一些进行异步操作控制的帮助工具，比如：

* ```mapLimit```，
* ```filterLimit```，
* ```concatLimit```，
* 以及 ```priorityQueue```。

如果你不打算重新发明轮子，不想把同样的逻辑自己再实现一遍，并且愿意信赖这个经过实践检验的、每月下载量高达 5000 万的库，你可以结合 ```util.promisify``` 简单地重用这些函数：

```javascript
const util = require('util')
const async = require('async')
const numbers = [
  1, 2, 3, 4, 5
]
mapLimitAsync = util.promisify(async.mapLimit)
async function main () {
  return await mapLimitAsync(numbers, 2, (number, done) => {
    setTimeout(function () {
      done(null, number * 2)
    }, 100)
  })
}
main()
  .then(console.log)
  .catch(console.error)
```

