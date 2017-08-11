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