# HTML 5.2 新特性介绍

> 本文译自 [What’s New in HTML 5.2?](https://bitsofco.de/whats-new-in-html-5-2/)
>
> 作者 [Ire Aderinokun](https://ireaderinokun.com/)，是一位前端开发者和 UI 设计师。

就在不到一个月之前，HTML 5.2 成为了 W3C 的官方推荐规范（REC），这意味着它得到了来自 W3C 主席及其成员的官方支持，W3C 组织将正式向浏览器厂商和 web 开发者推荐应用这一规范。

在 REC 阶段之前，[规范中的每一个新特性都应当已经有了至少两个独立实现](https://www.slideshare.net/rachelandrew/where-does-css-come-from/27?src=clipshare)。所以对于我们这些 web 开发者来说，现在已经可以开始实际应用这些新特性了。

要了解 HTML 5.2 中的所有变化之处，可以阅读这份[官方的文档](https://www.w3.org/TR/html52/changes.html#changes)。在这篇文章里，我会梳理一下我认为我的开发工作影响比较大的变化。

## 新特性

### 原生的 &lt;dialog&gt; 元素

在 HTML 5.2 的所有变化里我感到最为激动的就是引入了 [&lt;dialog&gt; 元素](https://www.w3.org/TR/html52/interactive-elements.html#elementdef-dialog)，实现了浏览器原生的对话框。对话框在 web 开发中非常常见，但是现在每个实现都不太一样。另一方面，实现一个支持无障碍化的对话框很难，实际上，现在 web 上使用的大多数对话框对于视觉障碍人士来说都是难以使用的。

新引入的 ```<dialog>``` 元素的目标就是要改变这一现状，让我们可以用一种简单的方式来实现一个标准的模态对话框，同时无须担心会有其它方面的问题。我会专门写一篇详细的文章介绍如何使用这个新元素，在这里我先做一个简单的说明。

首先，使用 ```<dialog>``` 元素可以创建一个对话框：

```html
<dialog>  
  <h2>对话框标题</h2>
  <p>对话框的内容写在这里</p>
</dialog>
```

默认情况下，对话框是不可见的，除非你设置了 ```open``` 属性。

```html
<dialog open>
```

```open``` 属性可以通过 ```HTMLDialogElement``` 上的 ```show()``` 和 ```close()``` 方法来改变。

```html
<button id="open">打开对话框</button>  
<button id="close">关闭对话框</button>

<dialog id="dialog">  
  <h2>对话框标题</h2>
  <p>对话框的内容写在这里</p>
</dialog>

<script>  
const dialog = document.getElementById("dialog");

document.getElementById("open").addEventListener("click", () => {  
  dialog.show();
});

document.getElementById("close").addEventListener("click", () => {  
  dialog.close();
});
</script>  
```

目前，Chrome 已经支持了 ```<dialog>``` 元素，而在 Firefox 中可以通过配置打开这一特性。具体情况可以查看 <https://caniuse.com/#feat=dialog>。

### 在 iframe 里使用支付请求 API

[支付请求 API](https://www.w3.org/TR/payment-request/) 是由浏览器原生提供支付方式，旨在为用户在 web 上进行支付提供一个标准而且一致的方法。它让浏览器提供统一一致的界面来搜集用户的支付信息，而不是让用户填写各个网站自己的支付表单。

在 HTML 5.2 之前，支付请求 API 不能在 iframe 中 使用。这使得那些第三方提供的嵌入式支付解决方案（例如 Stripe、Paystack）完全无法利用这个 API，因为它们的支付接口都是需要在一个 iframe 中进行处理的。

HTML 5.2 为 iframe 引入了一个 ```allowpaymentrequest``` 属性，设置这个属性就可以允许 iframe 中使用支付请求 API 了。

```html
<iframe allowpaymentrequest>
```

### 为苹果设备定义不同尺寸的图标

通过在 HTML 文档的头部使用 ```<link rel="icon">```，我们可以定义网页的图标。同时，还可以使用 ```sizes``` 属性来定义多个不同尺寸的图标。

```html
<link rel="icon" sizes="16x16" href="path/to/icon16.png">  
<link rel="icon" sizes="32x32" href="path/to/icon32.png">
```

虽然这个定义完全是建议性的，但它允许浏览器来自主决定使用哪个图标。尤其是像现在大多数设备的最优图标尺寸都不一样，只有浏览器自己才知道怎样的图标尺寸更为合适。

在 HTML 5.2 以前，```sizes``` 属性仅仅当 ```link``` 标签的 ```rel``` 属性为 ```icon``` 时才视为有效。可是，苹果的 iOS 设备并不支持这种 ```sizes``` 属性，它引入了一个私有的 rel 值  ```apple-touch-icon```，用于定义网页在苹果设备上的图标。

在 HTML 5.2 中，规范的这一限制被去除，当 ```rel``` 为 ```icon``` 或 ```apple-touch-icon``` 时都可以使用 ```sizes``` 属性。

## 新的有效写法

除了引入一些新特性，HTML 5.2 中也把一些之前被规范认为无效的 HTML 写法变成有效。

### 多个 &lt;main&gt; 元素

```<main>``` 元素用于表达网页的主体内容。对于在多个网页中会反复出现的内容，我们可以把它们放在 header、section 或者别的元素中，但 ```<main>``` 元素是被设计用于专门放置页面上特定且唯一的内容的。因此，在 HTML 5.2 之前，规范要求 ```<main>``` 元素在页面的 DOM 结构中只能出现一次。

可是随着单页应用的流行，我们难以再去坚持这一准则。可以设想会有这样一种情况：DOM 中有需要有多个 &lt;main&gt; 元素，但在同一时间用户只会看到其中一个。

在 HTML 5.2 中，现在只要能保证用户同时只能看到一个 ```<main>``` 元素，我们就可以在页面中多次使用这个标签。其它不显示的 ```<main>``` 元素必须通过 hidden 属性设置为隐藏。

```html
<main>...</main>  
<main hidden>...</main>  
<main hidden>...</main>  
```

我们都很清楚[利用 CSS 有多种办法可以隐藏元素](https://bitsofco.de/hiding-elements-with-css/)。可是对于页面上的多个 ```<main>``` 元素，我们必须用 hidden 属性将目前不需要显示的元素进行隐藏。任何别的方法，比如 ```display: none;``` 或者 ```visibility: hidden;```，都会被规范认为是无效的。

### 在 &lt;body&gt; 中定义样式

一般情况下，我们都会使用 ```<style>``` 标签来定义内联 CSS，并将其放置在 HTML  文档的 ```<head>``` 中。但随着组件化开发的兴起，开发者们开始逐渐倾向于把样式定义和与之相关的 HTML 元素放在一起。

在 HTML 5.2 中，在 ```<body>``` 中的任何地方都可以定义 ```<style>``` 块，规范现在将其也视为有效。也就是说我们现在可以让样式定义就出现在样式被使用的地方。

```html
<body>  
    <p>I’m cornflowerblue!</p>
    <style>
        p { color: cornflowerblue; }
    </style>
    <p>I’m cornflowerblue!</p>
</body>  
```

可是仍然需要注意的是，从性能角度考虑，样式定义最好还是放在 ```<head>``` 中。[规范](https://www.w3.org/TR/html52/document-metadata.html#elementdef-style)中也提到：

> 在文档的头部使用样式元素是一种更为可取的做法。在文档体内使用样式可能会导致页面样式重排、触发重新布局或（和）重绘，因此应该谨慎使用这一方式。

还应当注意的是，像在上面这个例子中，样式定义仍然是作用于全局的。在 HTML 文档内出现的样式定义仍然会应用于在其前面的元素之上，这也是它会造成重绘的原因。

### 在 &lt;Legend&gt; 内使用 h# 标签

```<legend>``` 用于在表单中表示一个 ```<fieldset>``` 的标题。在 HTML 5.2 之前，```<legend>``` 中的内容只能使用纯文本，现在我们可以在其中使用 h# 标签。

```Html
<fieldset>  
    <legend><h2>基本信息</h2></legend>
    <!-- 基本信息的表单域 -->
</fieldset>  
<fieldset>  
    <legend><h2>联系方式</h2></legend>
    <!-- 联系方式的表单域 -->
</fieldset>
```

当我们想使用 ```<fieldset>``` 来为表单中不同部分进行分组时，这一用法非常有用。就像上面这个例子，使用 h# 标签可以让那些依赖于文档大纲视图进行导航的用户更为方便地跳转到这些表单分组区域。

## 被移除的特性

在 HTML 5.2 中，有些特性被移除了，包括：

* keygen：用于为表单生成公钥
* menu 和 menuitem：用于创建导航或菜单

## 新的视为无效的写法

最后，还有一些开发实践被规范认为是无效的。

### &lt;p&gt; 元素中不允许包含行内、浮动或者块级元素

在 HTML 5.2 中，```<p>``` 元素只能包含短语内容（译者注：phrasing content，具体解释可参见[这里](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories)）。下列元素类型不能再被嵌套在一个段落中：

* 行内块
* 行内表格
* 浮动或者定位的块

### 不再需要严格的 Doctype 声明

我们终于可以和下面这种严格的文档类型说明说再见了：

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

