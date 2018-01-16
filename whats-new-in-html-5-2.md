# HTML 5.2 新特性介绍

就在不到一个月之前，HTML 5.2 成为了 W3C 的官方推荐标准（REC）。当一个标准达到了 REC 阶段，也就意味着它得到 W3C 成员们的官方支持，W3C 正式地向浏览器厂商和 web 开发者推荐应用这一标准。

在 REC 阶段之前，[标准中的每一个新特性都必须已经有了至少两个独立实现](https://www.slideshare.net/rachelandrew/where-does-css-come-from/27?src=clipshare)。所以对于我们这些 web 开发者来说，现在已经到了可以开始实际应用这些新特性的时候了。

要了解 HTML 5.2 中的所有变化之处，可以阅读这份[官方的文档](https://www.w3.org/TR/html52/changes.html#changes)。而在这篇文章里，我会介绍一些我认为会对我的开发工作影响比较大的一些变化。

## 新特性

### 原生的 &lt;dialog&gt; 元素

在 HTML 5.2 的所有变化里我感到最为激动的就是引入了 [&lt;dialog&gt; 元素](https://www.w3.org/TR/html52/interactive-elements.html#elementdef-dialog)，实现了浏览器原生的对话框。毫无疑问，对话框在 web 开发中非常常见，但是现在每个实现都不太一样。另一方面，对话框也很难做到无障碍化，实际上，现在 web 上使用的大多数对话框对于视觉障碍人士来说都是难以使用的。

新引入的 &lt;dialog&gt; 元素的目标就是要改变这一现状，通过一种简单的方式我们就可以实现一个标准的模态对话框，同时无须担心会有其它方面的隐患。我会专门写一篇详细的文章介绍如何使用这个新元素，在这里我先做一个简单的说明。

首先，使用 &lt;dialog&gt; 元素可以创建一个对话框：

```html
<dialog>  
  <h2>对话框标题</h2>
  <p>对话框的内容写在这里</p>
</dialog>
```

默认情况下，对话框是不可见的，除非你设置了 **open** 属性。

```html
<dialog open>
```

**open** 属性可以通过 **HTMLDialogElement** 上的 **show()** 和 **close()** 方法来改变。

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

目前，Chrome 已经支持了&lt;dialog&gt; 元素，而在 Firefox 中可以通过配置打开这一特性。具体情况可以查看 <https://caniuse.com/#feat=dialog>。

### 在 iframe 里使用支付请求 API

[支付请求 API](https://www.w3.org/TR/payment-request/) 是由浏览器原生的用于支付的解决方案，旨在为用户在 web 上进行支付提供一个标准的一致的方法。它让浏览器提供统一一致的界面来搜集用户的支付信息，而不是让用户填写各个网站自己的支付表单。

在 HTML 5.2 之前，支付请求 API 不能在 iframe 中 使用。这使得那些第三方提供的嵌入式支付解决方案（例如 Stripe、Paystack）完全无法利用这个 API，因为它们的支付接口都是需要在一个 iframe 中进行处理的。

HTML 5.2 为 iframe 引入了一个 **allowpaymentrequest** 属性，设置这个属性就可以允许在一个 iframe 中使用支付请求 API 了。

```html
<iframe allowpaymentrequest>
```

### 为苹果设备定义不同尺寸的图标

通过在 HTML 文档的头部使用 **&lt;link rel="icon"&gt;**，我们可以定义网页的图标。同时，还可以使用 **sizes** 属性来定义多个不同尺寸的图标。

```html
<link rel="icon" sizes="16x16" href="path/to/icon16.png">  
<link rel="icon" sizes="32x32" href="path/to/icon32.png">
```

虽然这个属性纯粹是建议性的，但它允许浏览器来决定使用哪个图标。尤其是像现在大多数设备的最优图标尺寸都不一样，只有浏览器自己才能更好地选择合适的图标尺寸。

在 HTML 5.2 以前，**sizes** 属性仅仅当 link 标签的 rel 属性为 icon 时才视为有效。可是，苹果的 iOS 设备并不支持 **sizes** 属性。它引入了一个私有的 rel 值 **apple-touch-icon**，用于定义网页在苹果设备上的图标。

在 HTML 5.2 中，规范的这一限制被去除，当 rel 为 icon 或 apple-touch-icon 时都可以使用 sizes 属性。

## 新的被认为有效的情况

除了