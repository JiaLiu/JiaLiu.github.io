# 在生产环境中直接部署 ES2015+ 代码
2017.09.13
> 本文译自 [Deploying ES2015+ Code in Production Today](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/)  
> 作者：Philip Walton

我接触过的大多数 web 开发者都表示在编写 JavaScript 代码时，非常喜欢使用那些最新的语法特性，比如 async/await、类、箭头函数等等。但是，时至今日，尽管所有的现代浏览器都已经可以原生支持上述特性，大多数开发者仍然要把他们的代码转译成 ES5，再与 plolyfills 打包在一起才能发布，而这样做的目的仅仅是为了适配那些只占很小比例的使用老式浏览器的用户。

这真的不是一种很好的做法，在理想情况下，我们不应该把实际上并不必要的代码打包到产品里去。

对于新的 JavaScript 和 DOM API，我们可以在运行时侦测当前浏览器对其是否支持，从而可以[按需加载 polyfills](https://philipwalton.com/articles/loading-polyfills-only-when-needed/)。但对于新的 JavaScript 语法，就比较不好办了，因为浏览器对于不认识的语法会报错，代码也就不会被执行。

目前我们确实还没有一个好的办法去精确侦测浏览器的对各种新语法的支持情况，但我们现在就有一个方法可以判断浏览器对一些基本的 ES2015 语法是否支持。

这个方法就是 ```<script type="module">```。

大多数开发者认为```<script type="module">```是一种加载 ES 模块的方式，当然这没错，但它还有一个更直接和实际的用途——可用于加载包含 ES2015+ 语法的普通 JavaScript 文件，并且能保证浏览器可以对其进行处理。

换句话说，只要是支持```<script type="module">```的浏览器必然也支持大部分你所喜爱的 ES2015+ 特性。例如：

* 支持 ```<script type="module">``` 的浏览器也支持 **async/await**
* 支持 ```<script type="module">``` 的浏览器也支持**类**
* 支持 ```<script type="module">``` 的浏览器也支持**箭头函数**
* 支持 ```<script type="module">``` 的浏览器也支持 **fetch**、**Promise**、**Map** 和 **Set** 等等特性

唯一需要额外处理的就是要提供一个回退机制，为那些不支持```<script type="module">```的浏览器提供一个不带有新语法特性的代码版本。幸运的是，如果你现在已经在为你的代码生成 ES5 版本的话，那么这件事你就已经做过了，而现在你需要做的就是再去生成一个 ES2015+ 版本。

在这篇文章里，我们将解释如何实现这项技术，讨论在生产环境中直接部署 ES2015+ 代码将会如何改变我们进行 npm 模块开发的方式。

## 如何实现
如果你现在已经在使用类似 webpack 或者 rollup 这样的打包工具来生成你的代码，那么你可以继续沿用这种方式。

接下来，除了你现在生成的包，你还需要采用类似的方式来生成第二个包；唯一的不同在于这一次你不再需要把代码转译成 ES5 了，也不要再捆绑 polyfills。

假如你已经在使用 [babel-preset-env](https://github.com/babel/babel-preset-env)（如果你还没用过，那么你应该试试它），那么这第二步就非常简单。你要做的就是把你的浏览器支持刘表改为那些支持 ```<script type="module">``` 的浏览器，这样 Babel 就会自动地不再进行不必要的代码转译。

也就是说，它会输出 ES2015+ 代码而不再是 ES5 代码。

举个例子：假设你正在使用 webpack，你的主文件入口是 ```./path/to/main.js```，那么你现在为了输出 ES5 版本将会这样配置 webpack（因为输出的是 ES5，所以这里我把输出包的名字写为 ```main-legacy```）：

```javascript
module.exports = {
  entry: {
    'main-legacy': './path/to/main.js',
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'public'),
  },
  module: {
    rules: [{
      test: /\.js$/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: [
            ['env', {
              modules: false,
              useBuiltIns: true,
              targets: {
                browsers: [
                  '> 1%',
                  'last 2 versions',
                  'Firefox ESR',
                ],
              },
            }],
          ],
        },
      },
    }],
  },
};
```
而如果要输出一个现代的 ES2015+ 版本的话，你只需要再添加一段配置，把你的目标环境设置为仅包含那些支持 ```<script type="module">``` 的浏览器即可：

```javascript
module.exports = {
  entry: {
    'main': './path/to/main.js',
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'public'),
  },
  module: {
    rules: [{
      test: /\.js$/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: [
            ['env', {
              modules: false,
              useBuiltIns: true,
              targets: {
                browsers: [
                  'Chrome >= 60',
                  'Safari >= 10.1',
                  'iOS >= 10.3',
                  'Firefox >= 54',
                  'Edge >= 15',
                ],
              },
            }],
          ],
        },
      },
    }],
  },
};
```
这样，运行 webpack 之后，这两个配置将会输出两个适用于生产环境的 JavaScript 文件：

* ```main.js```（ES2015+ 语法）
* ```main-legacy.js```（ES5 语法）

下一步则是更新你的 HTML，通过组合使用 ```<script type="module">``` 和 ```<script nomodule>``` 使得不同浏览器可以加载不同版本的文件：

```html
<!-- 支持 ES 模块的浏览器加载这个文件。 -->
<script type="module" src="main.js"></script>

<!-- 老式浏览器会加载这个文件 (支持模块的浏览器知道自己不应加载此文件）-->
<script nomodule src="main-legacy.js"></script>
```
> 注意：这里唯一存在的问题是 Safari 10 不支持 ```nomodule``` 属性，不过你可以通过在 ```<script nomodule>``` 前增加[一段 JavaScript 代码来解决这个问题](https://gist.github.com/samthor/64b114e4a4f539915a95b91ffd340acc)（这一问题在 Safari 11 中已被修复）。

### 要注意的地方
在大多数情况下，上述方法可以正常工作，但是在你真正实施这个方法之前，还有一些关于模块加载的重要细节需要了解：

1. 浏览器加载模块的方式类似于 [```<script defer> ```](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attr-defer)，直到整个文档处理结束之后才会执行模块文件。如果你的部分代码需要在此之前执行，那么你最好把这部分代码分离出来单独加载。
2. 模块中的代码必然是在 [stric mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) 模式下运行，所以如果由于某些原因你的代码需要在非严格模式下执行，那么这部分也需要单独加载。
3. 顶层声明的 ```var``` 和 ```function``` 在模块中的行为与在一般脚本中不同。例如，在普通脚本中的 ```var foo = 'bar'``` 和 ```function foo() {…}``` 可以通过 ```window.foo``` 来访问，而如果这样的代码出现在一个模块中则不能通过 ```window``` 访问它们。你需要确保你的代码不依赖于这种行为。

## 真实的例子
我创建了一个 [webpack-esnext-boilerplate](https://github.com/philipwalton/webpack-esnext-boilerplate) 项目，你可以看一看我是如何运用上述技术的。

在这个样板项目中，为了证明这项技术完全可以用于生产环境，我有意使用了几种高级的 webpack 特性，包括了一些打包时常见的推荐做法，比如像：

* [Code splitting](https://webpack.js.org/guides/code-splitting/)
* [Dynamic imports](https://webpack.js.org/guides/code-splitting/#dynamic-imports)（在运行时根据条件加载额外的代码）
* [Asset fingerprinting](https://webpack.js.org/guides/caching/)（便于实现更为有效的长期缓存）

同时，我推荐的东西我自己肯定也会先用起来，我已经在我的这个博客网站上使用了这项技术。你如果有兴趣可以查看其[源码](https://github.com/philipwalton/blog)。

即便你正在使用的打包工具不是 webpack，把这项技术运用到你项目中的做法其实也是大同小异。之所以我在这里使用 webpack 来进行演示，是因为 webpack 目前是最流行的打包工具，同时它也是同类工具中最复杂的。我觉得如果这项技术在 webpack 中可以使用，那它肯定也能与别的工具配合使用。

## 这些努力值得吗？
在我看来，答案是绝对值！其带来的改善是非常明显的。例如，我对我这个博客网站生成的代码文件总体积进行了一个比较：

|版本|体积（混淆后）|体积（混淆 + gzip压缩）|
|----|-----|-----|
|ES2015+ (main.js)|80K|**21K**|
|ES5 (main-legacy.js)|175K|**43K**|

传统的 ES5 版本的文件体积是 ES2015+ 版本的两倍还要多（甚至经过 gzip 压缩后也是如此）。

较大的文件会消耗更多的时间来下载，也会花更长的时间去处理和运行。比较我博客网站的两个版本文件在处理和运行时间方面的表现，ES5 版本也差不多是 ES2015+ 版本的两倍（这些测试是在 [webpagetest.org](webpagetest.org) 的一台 Moto G4 上进行的）：

|版本|处理/运行时间（单次运行）|处理/运行时间（平均值）|
|----|-----|-----|
|ES2015+ (main.js)|184ms，164ms，166ms|**172ms**|
|ES5 (main-legacy.js)|389ms，351ms，360ms|**367ms**|

当然，这些文件体积并不是很大、消耗的时间也不是特别长，这是因为我们只是测试了一个博客站点，它并没有加载太多的脚本文件。而现实中大多数网站的脚本文件规模不会这么小。你的脚本文件越多，你从 ES2015+ 上获得的收益也会越大。

你可能仍然会对此表示怀疑，认为文件体积和执行时间上的差别主要是来自于为了支持老式环境所必需的大量 polyfills，你这样的看法也不能说不对，捆绑 polyfills 确实是现在 web 上非常普遍的做法。

我对 [HTTPArchive 的数据做了一个快速的查询](https://bigquery.cloud.google.com/savedquery/438218511550:2cee796ae27f472fbfd517606a4bafc3)，结果表明在 Alexa 顶级排名网站中有 85181 个网站在其生产环境打包中包含了 [babel-polyfill](https://babeljs.io/docs/usage/polyfill/)、[core-js](https://github.com/zloirock/core-js) 或者 [regenerator-runtime](https://github.com/facebook/regenerator/blob/master/packages/regenerator-runtime/runtime.js)。而在六个月前这个数字是 34588！

事实表明，代码转写和打包 polyfills 正在迅速成为互联网的新常态。不幸的是，数百万用户正在通过网络电缆把数万亿字节非必要的数据传输给浏览器，其实这些浏览器早已能够很好地执行未转写的代码了。

## 我们应该开始发布 ES2015 版本的模块了
这项技术还需要面对的一个问题是，目前大多数 npm 模块作者不会发布他们的 ES2015 版本源代码，他们只发布经过转写的 ES5 版本。

时至今日，直接部署 ES2015+ 代码已经成为可能，到了我们改变这一现状的时候了。

当然，我也知道如果我们这样做，在不远的将来会带来很多挑战。现今的大多数编译工具在其文档中[推荐的配置](https://github.com/babel/babel-loader/blob/v7.1.2/README.md#usage)都[假设所有的模块是 ES5 版本的](https://rollupjs.org/#babel)。这意味着如果模块作者开始在 npm 上发布 ES2015+ 版本，会很有可能[导致一些用户的编译过程出现错误](https://github.com/googleanalytics/autotrack/issues/137)，从而把用户搞得很混乱。

这个问题的原因在于大多数使用 Babel 的开发者都会配置不让 Babel 转写 ```node_modules``` 下面的文件，但如果模块都以 ES2015+ 版本发布，这样做就会有问题。幸运的是修正这个问题非常简单。你只要在你的配置中不要排除 ```node_modules``` 即可：

```javascript
rules: [
  {
    test: /\.js$/,
    exclude: /node_modules/, // 删除这一行
    use: {
      loader: 'babel-loader',
      options: {
        presets: ['env']
      }
    }
  }
]
```
这样做有一个缺点就是 Babel 会转写 ```node_modules``` 下的所有模块，而不仅限于我们真正需要的那些依赖，这样会导致编译过程变得缓慢。幸运的是这个问题已经可以通过[持久化的本地缓存机制](https://github.com/babel/babel-loader/blob/v7.1.2/README.md#babel-loader-is-slow)来解决。

在推动 ES2015+ 成为新的模块发布标准的道路上，无论我们遇到怎样的障碍，我认为那都是值得我们去努力跨越的。假如我们这些写模块的人一直都在 npm 上只发布 ES5 版本，那其实就是我们在强迫用户使用臃肿而缓慢的代码。

通过发布 ES2015 版本的模块，我们给使用模块的开发者一个选择的机会，最终会让所有人受益。

## 结论
```<script type="module">``` 本来是用于在浏览器中加载 ES 模块及其依赖的，但这并不妨碍我们用它来做点别的事情。

```<script type="module">``` 完全可以用于加载一个单独的 JavaScript 文件，从而实现在支持现代语法特性的浏览器中加载对应版本的脚本文件，这正是开发者们所迫切需要的一种解决方案。

同时再辅以 ```nomodule``` 属性，我们就可以在生产环境中使用 ES2015+ 代码，从而让我们不必在不必要的时候还是向浏览器传输大量的转写代码。

用 ES2015 来编写程序只是开发者的福音，而部署 ES2015 代码才会让让用户最终受益。

## 进阶阅读
* [ES6 Modules in Chrome M61+](https://medium.com/dev-channel/es6-modules-in-chrome-canary-m60-ba588dfb8ab7)
* [ECMAScript modules in browsers](https://jakearchibald.com/2017/es-modules-in-browsers/)
* [ES6 Modules in Depth](https://ponyfoo.com/articles/es6-modules-in-depth)