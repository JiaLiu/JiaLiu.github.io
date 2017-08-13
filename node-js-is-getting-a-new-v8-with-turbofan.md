# 新 V8 带来的 NODE.JS 性能变化

2017-07-27

V8 的 TurboFan 将会如何影响我们的代码优化方式

> 作者：[David Mark Clements](https://www.nearform.com/author/david-mark-clements)  
> 原文：[GET READY: A NEW V8 IS COMING, NODE.JS PERFORMANCE IS CHANGING](https://www.nearform.com/blog/node-js-is-getting-a-new-v8-with-turbofan/)

![Turbofan](https://www.nearform.com/img/blog/2017/07/turbofan.png)

原文经过了来自 V8 团队的 [Franziska Hinkelmann](https://twitter.com/fhinkel) 和 [Benedikt Meurer](https://twitter.com/bmeurer) 的审阅。

Node.js 从它诞生的第一天开始，就依赖于 V8 JavaScript 引擎来执行我们的 JavaScript 代码。V8 引擎是由 Google 打造的 JavaScript 虚拟机，用在它的 Chrome 浏览器中。从一开始，V8的主要目标就是让 JavaScript 执行地更快，或者至少要胜过其它竞争者。对于 JavaScript 这样一个具有高度动态性、弱类型语言而言，达到这个目标绝非易事。让我们先回顾一下 V8 和 JS 引擎性能的发展历史。

让 V8 引擎可以快速执行 JavaScript 的核心要素是 JIT（Just In Time）编译器。它是一个可以在运行时优化代码的动态编译器。V8 最早设计的 JIT 编译器被取名为 FullCodegen，随后 V8 团队又实现了 Crankshaft，其中实现了许多 FullCodegen 中所没有的性能优化。

我从上世纪 90 年代就开始观察和使用 JavaScript，我感到似乎无论在哪个 JavaScript 引擎中，代码的执行速度经常是反直觉的，面对一些明显执行很慢的 JavaScript 代码我们却很难知道其背后的原因。

近几年来，我和 [Matteo Collina](https://twitter.com/matteocollina) 致力于研究怎样写出高效的 Node.js 代码。经过这些年的努力，我们已经知道了在 V8 引擎上，怎样就能写出执行比较快的代码，而什么样的代码执行起来会比较慢。

但现在，挑战我们现有认知的时刻到了，因为 V8 团队推出了一个新的 JIT 编译器：Turbofan。

有一些大家众所周知的代码编写方式会导致执行效率低下（尽管在 Turbofan 中可能并非如此），我和 Matteo 在进行 Crankshaft 性能研究过程中也有一些更鲜为人知的发现。在这篇文章里，我们将讨论所有这些点，通过一系列性能基准测试，来看一看各版本 V8 的变化。

当然，在为 V8 优化我们的代码之前，我们应该首先把精力放在 API 设计、算法和数据结构上。本文中的性能测试仅仅是为了比较在 Node 中 JavaScript 运行效率的变化。我们当然可以据此来改变我们编写代码的方式以改善代码运行的效率，但是在这之前，我们应该首先使用一些更为通用的优化手段。

接下来我们将看一下 V8 5.1、5.8、5.9、6.0 和 6.1 的性能测试情况。

V8 5.1 是 Node 6 使用的引擎，包含了 Crankshaft JIT 编译器；V8 5.8 则用于 Node 8.0 到 8.2，使用的是 Crankshaft 和 Turbofan 的混合体。

V8 6.0 被包含在 Node 8.3（也可能是 8.4）里，而写作本文时最新的 V8 版本是 6.1，它被整合在 node-v8 的试验仓库（<https://github.com/nodejs/node-v8>）中。换句话说，V8 6.1 最终将会出现在 Node 未来的某个版本中，可能是 Node.js 9。

在呈现测试结果的同时，我们也会讨论这些变化对将来意味着什么。这些基准测试都是用 [benchmark.js](https://www.npmjs.com/package/benchmark) 运行的，其中的值反映的是每秒钟的执行次数，因此在每幅图表中，结果值越高意味着性能越好。

## Try/Catch
有一个大家众所周知的反优化模式就是使用```try/catch```。

在这个测试中我们对 1 到某个自然数进行累加求和，比较了四种不同编码方式的性能：

* 把累加过程包含在一个```try/catch```中（sum with try catch）
* 不对累加过程进行```try/catch```（sum without try catch）
* 把累加过程写成一个函数，再在一个```try```块中执行它（sum wrapped）
* 把累加过程写成一个函数，然后简单地直接执行它（sum function）

代码：<https://github.com/davidmarkclements/v8-perf/blob/master/bench/try-catch.js>

![](https://www.nearform.com/img/blog/2017/07/try-catch-bar.png)

从结果可见，我们原本对于```try/catch```会导致性能问题的观点在 Node 6（V8 5.1）上还是正确的，但是```try/catch```对性能的影响在 Node 8.0-8.2（V8 5.8）上已经显著降低。

同时也要注意，在一个```try```块内执行一个函数要比在```try```块外执行它要慢得多，这一点在 Node 6（V8 5.1）和 Node 8.0-8.2（V8 5.8）是一致的。

但是对于 Node 8.3+，在```try```块内执行函数导致的性能下降却几乎可以忽略不计了。

尽管如此，也不要以为万事大吉。我和 Matteo 在为一些性能研讨会准备材料的时候，[发现了一个 V8 性能 bug](https://bugs.chromium.org/p/v8/issues/detail?id=6576&q=matteo%20collina&colspec=ID%20Type%20Status%20Priority%20Owner%20Summary%20HW%20OS%20Component%20Stars)：某种特定的代码逻辑组合可能会导致 Turbofan 陷入到一个反优化/再优化的无穷循环中去，从而导致性能下降。

## 从对象中移除属性
多年以来，想要写出高性能 JavaScript 代码的人都不会使用```delete```操作符（或至少在我们试图优化一些高频代码时，我们会避免使用```delete```）。

```delete```的问题根源在于 V8 对 JavaScript 对象的动态特性和原型链（也具有潜在的动态性）的处理方式。这些动态特性使得在引擎在实现对属性的查找时变得异常复杂。

V8 引擎为了提高属性和对象的处理速度，在 C++ 层面基于对象的“形状”为对象创建了 C++ 类。所谓“形状”，就是指对象中每个属性的键和值（也包括原型链上的键和值）；而这些类就被称为“hidden classes”。可是这种优化是发生在运行时的，如果一个对象的“形状”是不确定的，那么 V8 就会使用另一种慢地多的方式来进行属性获取：查找哈希表。一直以来，当我们从对象中```delete```一个属性之后，后续的属性查找模式就会变成哈希表查找。这就是我们要避免使用```delete```的原因。我们会把要移除的属性赋值为```undefined```来达到类似的效果，在大多数情况下这么做完全可以满足需要了，除了可能会在检查属性是否存在时会有点问题。```JSON.stringify```也不会在其输出结果中包含```undefined```值（按照 JSON 规范，```undefined```并不是一个有效的值），所以这样做对于对象序列化也没有问题。

现在，让我们看一下新的 Turbofan 是否解决了```delete```问题。

在这个测试中我们比较了三种情况：

* 把对象中的一个属性设为```undefined```，然后序列化这个对象（setting to undefined）
* 使用```delete```来移除一个属性，然后序列化这个对象，被删除的属性不是这个对象中最后加入的属性（delete）
* 使用```delete```来移除一个属性，然后序列化这个对象，被删除的属性是这个对象中最近新加入的属性（delete last property）

代码：<https://github.com/davidmarkclements/v8-perf/blob/master/bench/property-removal.js>

![](https://www.nearform.com/img/blog/2017/07/property-removal-bar.png)

在 V8 6.0 和 6.1 上，删除最后添加的属性变快了很多，甚至和“设为 undefined”性能差不多。这真是一个很好的消息，它说明 V8 团队正在努力解决```delete```带来的性能问题。可是对于那些并非新添加的最后一个属性，```delete```仍然会导致性能下降。因此总的来说，我们仍然不得不继续建议大家不要使用```delete```。

## 数组化还是泄露 arguments
```arguments```是一个在一般 JavaScript 函数（而箭头函数就没有```arguments```对象）中存在的隐式对象，由于它像是数组但又不是数组，在使用时经常会碰到问题。

为了能够对```arguments```对象使用数组方法，像数组一样地使用它，我们需要把它的每个索引属性都复制到一个真正的数组中。在过去，JavaScript 开发人员倾向于认为代码越少则速度越快。这个粗糙的规则对于运行在浏览器中的代码确实会带来在传输和加载体积方面的好处，但在服务器端，代码执行速度的重要性远远大于代码的体积，仍然套用这个规则可能会导致问题。所以，很多人都习惯使用一种看上去很巧妙且简便的方式来把```arguments```对象转化为数组：```Array.prototype.slice.call(arguments)```。通过执行数组的```slice```方法，把```arguments```对象作为执行时的上下文即```this```传入，```slice```方法会把传入的冒牌货当做一个数组来操作。于是，我们可以得到一个包含了整个参数对象成员的数组。

可是，把一个函数的隐式```arguments```对象从函数上下文中暴露出去（比如把它通过返回值输出到函数之外，或者正如在```Array.prototype.slice.call(arguments)```一样，```arguments```对象被传送给了另一个函数）会导致性能下降。现在让我们看看是不是会这样。

下面这个测试基于我们的四个 V8 版本，比较了两个相互关联的主题：对外泄露```arguments```的代价和把 arguments 复制为一个数组（然后再以同样的方式传送到函数之外）的代价：

* 把```arguments```对象暴露给另外一个函数（leaky arguments）
* 使用```Array.prototype.slice```技巧把```arguments```复制为一个数组（Array prototype.slice arguments）
* 使用 for 循环复制```arguments```中每个属性（for-loop copy arguments）
* 使用 ES2015 中的 spread 操作符来转化```arguments```为一个数组（spread operator）