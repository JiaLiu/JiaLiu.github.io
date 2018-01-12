# JavaScript 核心概念：第二版

这篇文章是 [JavaScript 核心概念](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/)的更新版本，在这里我们主要讨论的是 ECMAScript 语言及其运行时的核心组件。

本文适合有一定经验的开发者和从业人员阅读。

在这篇文章的[前一个版本](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/)中我曾经介绍了 JS 语言的基本要素，其中主要使用的是当时 ES3 规范中的概念抽象，也涉及到一些在 ES5 和 ES6（也就是 ES2015）中的相关变化。

从 ES2015 开始，规范改变了一些核心组件的描述和结构，引入了新的模型。在这里，我们会关注这些比较新的概念以及有所变化的术语；而对于那些在各版本规范中都保持兼容的 JS 基础结构，我们也会继续保留在这篇文章里。

本文已经涵盖了ES2017+ 的运行时系统。

> 在 TC-39 网站上你可以找到最新版的 [ECMAScript 规范](https://tc39.github.io/ecma262/)。

我们从**对象**的概念开始我们的讨论，对象是 ECMAScript 的基础。

## 对象（Object）

ECMAScript 是一门**基于原型**的**面向对象**的编程语言，它以**对象**的概念作为其核心抽象。

> **定义 1 **
>
> **对象**：一个对象就是一个**属性的集合**，并且拥有一个**单独的原型对象**。这个原型要么是一个对象，要么就是 ```null``` 值。

一个对象的内部属性 ```[[Prototype]]``` 指向的就是它的原型，在代码中通过 ```__proto__``` 属性暴露出来。以这段代码为例：

```javascript
let point = {
  x: 10,
  y: 20,
};
```

这个对象拥有两个**显式的自身属性**和一个**隐含的** ```__proto__``` 属性， ```__proto__``` 就是一个指向 ```point``` 对象原型的引用：

![一个具有原型的基本对象](http://dmitrysoshnikov.com/wp-content/uploads/2017/11/js-object.png "一个具有原型的基本对象")

图 1 一个具有原型的基本对象

> 对象也可以存储**符号（symbol）**。你可以通过[这篇文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)了解更多关于符号的信息。

原型对象被用于实现基于**动态分派**的**继承**机制。下面我们要了解一下**原型链**的概念以及详细介绍这种继承机制的细节。

## 原型（Prototype）

每个对象在创建时都会拥有一个**原型**。如果创建时没有**显式**地设置对象的原型，那这个对象就会有一个**默认的原型**。

> **定义 2**
>
> **原型**：**原型**是一个用于实现**原型继承**的委托对象。

原型可以通过 ```__proto__``` 属性或者 ```Object.create``` 方法**显式**设置：

```javascript
// 基本对象
let point = {
  x: 10,
  y: 20,
};
 
// 从 point 对象继承.
let point3D = {
  z: 30,
  __proto__: point,
};
 
console.log(
  point3D.x, // 10，继承属性
  point3D.y, // 20，继承属性
  point3D.z  // 30，自身属性
);
```

> 默认情况下，对象会把 ```Object.prototype``` 作为其原型对象。

任何对象都可以作为其它对象的原型，原型对象本身也会有自己的原型。如果原型的原型再有自己的原型，以此类推，就会形成所谓的**原型链**。

> **定义 3**
>
> **原型链**：**原型链**是一个**有限**的对象链条，用于实现**继承**和**属性共享**。

![原型链](http://dmitrysoshnikov.com/wp-content/uploads/2017/11/prototype-chain.png "原型链")

图 2 原型链

按照原型链查询对象属性的规则非常简单：如果一个属性不是对象的自身属性，就会尝试去到它的原型上去查询，以此类推直到遍历完整个原型链。

技术上这种机制被称为**动态分派（dynamic dispatch）**或者**委托(delegation)**。

> **定义 4**
>
> **委托**：一种用于在原型链中解析属性的机制。这一过程发生在运行时，因此也被叫做**动态分派**。

> 如果解析是发生在**编译时**，就称为**静态分派（static dispatch）**；而动态分派则是在**运行时**解析属性的引用。

 如果一个属性在原型链中最终都没有找到，那么就会返回 ```undefined``` 值：

```javascript
// 一个“空”对象。
let empty = {};
 
console.log(
 
  // 这个函数继承于默认原型
  empty.toString,
   
  // undefined
  empty.x,
 
);
```

可见，一个默认的对象实际上也**并不**是空的 —— 它总是会从 ```Object.prototype``` 继承**一些东西**。如果只是想创建一个对象来作为字典使用，不需要有默认原型，那我们必须显式地把它的原型设为 ```null```：

```javas
// 不要继承任何东西
let dict = Object.create(null);
 
console.log(dict.toString); // undefined
```

动态分派机制允许修改继承链，可以改变一个对象的原型：

```javascript
let protoA = {x: 10};
let protoB = {x: 20};
 
// 也可以写作 ”let objectC = {__proto__: protoA};“：
let objectC = Object.create(protoA);
console.log(objectC.x); // 10
 
// 改变原型：
Object.setPrototypeOf(objectC, protoB);
console.log(objectC.x); // 20
```

> 尽管现在 ```__proto__``` 属性已经被纳入规范，而且用起来也很方便，但在实际编码中还是更推荐使用 API 的方式进行有关于原型的操作，例如 ```Object.create```、```Object.getPrototypeOf```、```Object.setPrototypeOf```，以及在 ```Reflect``` 模块中的一些类似方法。

对象的默认原型都是 ```Object.prototype```，由此可见**多个对象**可以共享**同一个原型**。基于这一点，ECMAScript 实现了其**类继承**机制。下面我们将了解一下 JS 中 ”类“这一抽象的原理是什么。

## 类（Class）

当几个对象共享相同的初始状态和行为，那么它们就形成一个**分类**。

> **定义 5**
>
> **类**：一个**类**就是一个正式的抽象集合，它定义了其对象的初始状态与行为。

当我们需要有**多个对象**继承自同一个原型，我们当然可以把这个原型对象先创建出来，然后通过显式继承来创建多个对象：

```javascript
// 所有字母的原型。
let letter = {
  getNumber() {
    return this.number;
  }
};
 
let a = {number: 1, __proto__: letter};
let b = {number: 2, __proto__: letter};
// ...
let z = {number: 26, __proto__: letter};
 
console.log(
  a.getNumber(), // 1
  b.getNumber(), // 2
  z.getNumber(), // 26
);
```

下面这幅图显示了他们之间的关系：

![](http://dmitrysoshnikov.com/wp-content/uploads/2017/11/shared-prototype.png)

图 3 原型共享

可是这样做明显有些繁琐。而类抽象——作为一种语法糖（就是一种在语义上并无区别、但在句法形式上好看地多的结构）——恰恰就是为这种使用场景而生。利用类，我们可以用一种更简便的方式来创建多个对象：

```javascript
class Letter {
  constructor(number) {
    this.number = number;
  }
 
  getNumber() {
    return this.number;
  }
}
 
let a = new Letter(1);
let b = new Letter(2);
// ...
let z = new Letter(26);
 
console.log(
  a.getNumber(), // 1
  b.getNumber(), // 2
  z.getNumber(), // 26
);
```

> 在 ECMAScript 中，**类继承**背后就是通过**原型委托**来实现的。

> **类**只是一个理论上的抽象概念。在技术上，它既可以像在 Java 或者 C++ 语言中那样通过**静态分派**的方式实现，也可以采用**动态分派（委托）**的机制，例如在 JavaScript、Python 或者 Ruby 等语言中的实现。

技术上，一个“类”通常被表达为“构造函数 + 原型”的组合。因此，构造函数会**创建对象**，也会**自动**为其新创建的对象设置**原型**。这个原型就存储在 ```<ConstructorFunction>.prototype``` 属性中。

> **定义 6**
>
> **构造函数**：就是一个用于创建实例并自动为实例设置原型的函数。

我们完全可以显式地使用构造函数。而且，在类抽象概念被引入之前，JS 开发者都习惯于这样做，因为他们当时并没有更好的替代方案（在互联网上我们也仍然可以找到很多这样的旧式代码）：

```javascript
function Letter(number) {
  this.number = number;
}
 
Letter.prototype.getNumber = function() {
  return this.number;
};
 
let a = new Letter(1);
let b = new Letter(2);
// ...
let z = new Letter(26);
 
console.log(
  a.getNumber(), // 1
  b.getNumber(), // 2
  z.getNumber(), // 26
);
```

尽管创建一个单一的构造函数看上去也十分地简单，但这种继承方式意味着我们需要写不少模板式的代码。而现在，通过在 JavaScript 语言中引入类概念，这种模板已经被作为实现细节隐藏了起来。

> 构造函数就是类继承背后的实现细节。

让我们来看一下对象和它们的类的关系：

![](http://dmitrysoshnikov.com/wp-content/uploads/2017/11/js-constructor.png)

图 4 构造函数与对象之间的关系

这幅图反映出**每一个**对象都有原型。即便是像构造函数（类）```Letter```，也有 ```Function.prototype``` 作为自己的原型。注意，```Letter.prototype``` 是 Letter 实例 ```a```、```b``` 和 ```z``` 的原型。

> 任何对象的实际原型都是其 ```__proto__``` 引用。而构造函数上的显式 ```prototype``` 属性则是一个指向其实例的引用，也就是说，实例的 ```__proto__``` 属性指向它。如果想了解更多细节，可以查看[这里](http://dmitrysoshnikov.com/ecmascript/chapter-7-2-oop-ecmascript-implementation/#explicit-codeprototypecode-and-implicit-codeprototypecode-properties)。

你可以在 [ES3.7.1 面向对象编程：基本理论](http://dmitrysoshnikov.com/ecmascript/chapter-7-1-oop-general-theory/)中了解到关于面向对象编程基本概念的深入讨论（包括对类、原型等概念的深入阐述）。

现在我们已经理解了 ECMAScript 对象之间的基本关系，接下来让我们将讨论 JS 的运行时系统。我们将会看到其中任何东西几乎都可以被表达为一个对象。