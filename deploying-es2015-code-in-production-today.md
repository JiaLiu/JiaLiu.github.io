# 在生产环境中部署 ES2015+ 代码
2017.09.13
> 本文译自 [Deploying ES2015+ Code in Production Today](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/)  
> 作者：Philip Walton

我接触过的大多数 web 开发者都表示在编写 JavaScript 代码时，非常喜欢使用那些最新的语法特性，比如 async/await、类、箭头函数等等。但是，时至今日,尽管所有的现代浏览器都已经可以原生支持上述特性，大多数开发者仍然要把他们的代码转译成 ES5，再与 plolyfills 打包在一起才能发布，这样做的目的却仅仅是为了适应那些只占很小比例的老式浏览器用户。

这真的很差劲，在理想情况下，我们不应该把实际上并不必要的代码打包到产品里去。

对于新的 JavaScript 和 DOM API，我们可以在运行时侦测当前浏览器对其是否支持，从而可以[按需加载 polyfills](https://philipwalton.com/articles/loading-polyfills-only-when-needed/)。但对于新的 JavaScript 语法，就比较不好办了，因为浏览器对于不认识的语法会报错，代码也就不会被执行。

虽然目前我们还没有好的办法去侦测浏览器的语法支持情况，但我们现在确实有一个办法去侦测浏览器是否支持基本的 ES2015 语法。

这个方法就是```<script type="module">```。

大多数开发者认为```<script type="module">```是一种加载 ES 模块的方式，当然这没错，但它还有一个更直接和实际的用途——就是加载包含 ES2015+ 语法的普通 JavaScript 文件，并且可以确保浏览器能够进行正常处理。

换句话说，只要是支持```<script type="module">```的浏览器必然也支持大部分你所喜爱的 ES2015+ 特性。例如：

* 支持```<script type="module">```的浏览器也支持 **async/await**
* 支持```<script type="module">```的浏览器也支持**类**
* 支持```<script type="module">```的浏览器也支持**箭头函数**
* 支持```<script type="module">```的浏览器也支持  **fetch**、**Promise**、**Map** 和 **Set** 等等特性

唯一一件需要额外做的事情是作为回退机制，要给那些不支持```<script type="module">```的浏览器提供一个不带有新语法特性的代码版本。幸运的是，如果你现在已经在为你的代码生成 ES5 版本的话，那么这件事你已经做了。现在你需要做的就是再去生成一个 ES2015+ 版本。

在这篇文章里，我们将解释如何实现这项技术，讨论在生产环境中直接部署 ES2015+ 代码将会如何改变我们编写模块的方式。

## 如何实现
如果你现在已经在使用类似 webpack 或者 rollup 这样的打包工具来生成你的代码，那么请继续沿用这种方式。

接下来，除了你现在生成的包，你还需要采用类似的方式来生成第二个包；唯一的不同在于这一次你不再需要把代码转译成 ES5 了，也不要再捆绑 polyfills。

假如你已经在使用 [babel-preset-env](https://github.com/babel/babel-preset-env)（如果你还没用过，那么你应该试试它），那么这第二步就非常简单。你要做的就是把你的浏览器支持刘表改为那些支持```<script type="module">```的浏览器，这样 Babel 就会自动地不再进行不必要的代码转译。

也就是说，它会输出 ES2015+ 代码而不再是 ES5 代码。