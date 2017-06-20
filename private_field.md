# JavaScript 的新特性 —— 类的私有属性
*这是什么，如何使用以及它为什么会被设计成这样子*
***
[类的私有属性](https://github.com/tc39/proposal-class-fields#private-fields)这一新特性目前正处于 JavaScript 标准流程的 [Stage 2](https://tc39.github.io/process-document/) 阶段。尽管尚未最终确定，但 JavaScript 标准委员会期望它最后能被纳入到标准之中（期间仍然可能会有一些变化）。

其语法（现阶段）就像这样
```javascript
class Point {
  #x;
  #y;

  constructor(x, y) {
    this.#x = x;
    this.#y = y;
  }

  equals(point) {
    return this.#x === point.#x && this.#y === point.#y;
  }
}
```
其中主要涉及两个方面：
1. 如何定义私有属性
2. 如何引用私有属性

## 定义私有属性
私有属性的定义方式几乎与公有属性完全一致：
```javascript
class Foo {
  publicFieldName = 1;
  #privateFieldName = 2;
}
```
在访问一个私有属性之前，你必须首先定义它。而如果你不想在定义这个属性的同时就给它一个初始值，你也可以这样做：
```javascript
class Foo {
  #privateFieldName;
}
```
## 引用私有属性
引用私有属性也和访问其它属性非常类似，唯一不同之处是其特别的语法：
```javascript
class Foo {
  publicFieldName = 1;
  #privateFieldName = 2;
  add() {
    return this.publicFieldName + this.#privateFieldName;
  }
}
```
`this.#` 还有一种简略的写法：
```javascript
method() {
  #privateFieldName;
}
```
这段代码与下面代码的含义是一样的：
```javascript
method() {
  this.#privateFieldName;
}
```
## 引用类实例的私有属性
不仅可以利用 `this` 来引用自己的私用属性，你也可以在类中访问同类其它实例的私有属性：
```javascript
class Foo {
  #privateValue = 42;
  static getPrivateValue(foo) {
    return foo.#privateValue;
  }
}

Foo.getPrivateValue(new Foo()); // >> 42
```
在这段代码中，`foo` 是 `Foo` 类的一个实例，因此我们可以在 `Foo` 的类定义中访问 `foo` 的私有属性 `#prvateValue`。
## 私有方法（即将到来？）
有关私有属性的[提案](https://github.com/tc39/proposal-class-fields)仅仅涉及了为类增加一种新的属性，却并未对类方法做出任何改变。类的私有方法将会出现在[随后的另一份提案](https://github.com/tc39/proposal-private-fields/blob/master/METHODS.md)中，它可能会是这样：
```javascript
class Foo {
  constructor() {
    this.#method();
  }
  #method() {
    // ...
  }
}
```
同时你仍可以将函数赋值给私有属性：
```javascript
class Foo {
  constructor() {
    this.#method();
  }

  #method = () => {
    // ...
  };
}
```
## 封装
你不能通过一个类的实例访问它内部的私有属性，除非你是在定义这些私有属性的类内部访问它们。
```javascript
class Foo {
  #bar;
  method() {
    this.#bar; // 没有问题
  }
}
let foo = new Foo();
foo.#bar; // 无效!
```
并且，作为一个真正的私有属性，你甚至也无法侦测到它的存在。为了保证这一点，我们需要允许公有属性和私有属性可以具有相同的名字。
```javascript
class Foo {
  bar = 1; // 公有属性 bar
  #bar = 2; // 私有属性 bar
}
```
如果不允许私有属性与公有属性同名，那你就可以通过对属性赋值的方式来侦测私有属性是否存在：
```javascript
foo.bar = 1; // Error: `bar` is private! （侦测成功！）
```
或者换一个“安静”些的版本：
```javascript
foo.bar = 1;
foo.bar; // `undefined` (侦测成功！)
```
这种封装性同样也体现在子类中。子类的属性可以与父类中的属性同名。
```javascript
class Foo {
  #fieldName = 1;
}

class Bar extends Foo {
  fieldName = 2; // 没有问题
}
```
如果想了解更多这种封装性设计背后的动机，可以参考[这部分内容](https://github.com/tc39/proposal-private-fields/blob/master/FAQ.md#why-is-encapsulation-a-goal-of-this-proposal)。
## 为什么需要符号# 
许多人都会想知道，为什么不参照许多其它语言的惯例，使用 `private` 关键字来标识私有属性？例如：
```
class Foo {
  private value;

  equals(foo) {
    return this.value === foo.value;
  }
}
```
让我们还是从两个方面来看一下这个问题。
### 为什么不使用 `private` 关键字来声明私有属性
在很多不同的程序语言里，都使用 `private` 关键字声明私有属性。
让我们来看一下这种语法：
```
class EnterpriseFoo {
  public bar;
  private baz;
  method() {
    this.bar;
    this.baz;
  }
}
```
在这些语言中，公有属性和私有属性的访问方式是一致的，因而它们采用这种定义方式也是合理的。

可是在 JavaScript 中，由于我们不能使用 `this.field` 的方式访问一个私有属性（我会在后文讨论这个原因），因此我们需要一种句法上的形式来统一私有属性的声明与访问。通过在这两个地方都使用符号#，到底在引用哪个属性就会清晰地多。
### 为什么引用属性时需要符号#？
我们需要用 `this.#field` 而不是 `this.field` 有如下这些原因：
1. 正如在上文“封装”一节中所讨论的，我们需要允许公有属性和私有属性同名，因此不能采用过去的传统方式去访问一个私有属性。
2. 在 JavaScript 中可以采用 `this.field` 或者 `this['field']` 的方式引用公有属性。而由于私有属性是静态的（不能动态添加），它不能支持第二种引用方式。这可能会导致语法上的混乱。
3. 会承担额外的检查“代价”。

关于第三点，让我们来看一个例子：
```javascript
class Point {
  #x;
  #y;

  constructor(x, y) {
    this.#x = x;
    this.#y = y;
  }
  equals(other) {
    return this.#x === other.#x && this.#y === other.#y;
  }
}
```
请注意这里我们是如何引用 `other` 的私有属性 `#x` 和 `#y` 的。这种方式意味着我们假设 `other` 必然是 `Point` 类的一个实例。

因为我们使用了符号#这种语法，相当于我们已经告诉 JavaScript 编译器我们正在查询的是当前类的私有属性。

但如果我们不使用这个符号会发生什么呢？
```javascript
equals(otherPoint) {
  return this.x === otherPoint.x && this.y === otherPoint.y;
}
```
现在我们就有了一个问题：我们怎样才能知道 `otherPoint` 是什么？

JavaScript 没有静态类型系统，因此 `otherPoint` 可能是任何东西。

之所以这是一个问题有两个原因：
1. 依赖于你传入值的类型，我们的函数将会产生不同的行为：有时在访问一个私有属性，有时又会去访问一个公有属性。
2. 我们将不得不每次都去检查 `otherPoint` 的类型：
```javascript
if (
  otherPoint instanceof Point &&
  isNotSubClass(otherPoint, Point)
) {
  return getPrivate(otherPoint, 'foo');
} else {
  return otherPoint.foo;
}
```
甚至更糟的是，我们将不得不每次在类中访问一个属性时都去检查我们是否在引用一个私有属性。

属性的访问已经很慢了，我们绝对不能再让这个操作变得更慢了。

总之，我们需要使用符号#来标识私有属性，而使用其它方式会造成不可预期的行为和结果，并带来巨大的性能问题。  
***
私有属性对语言来说是一个非常好的补充。感谢 TC39 委员会中所有曾经及现在为之付出卓越努力的人们！
***
本文的英文原文基于 [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/) 授权。 
