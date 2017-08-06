# Web 框架：基础技术

> 本文译自：[Web Frameworks: 
Foundational Technologies](https://www.sitepen.com/blog/2017/07/06/web-frameworksfoundational-technologies/)  
作者：[Kit Kelly](https://www.sitepen.com/blog/author/kkelly/)

![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/blog-7.jpg)

在之前的文章里我们讨论了一些 web 框架的 “look & feel“，也就是用它们如何进行用户界面开发与定义样式。我们经常因为一些组件和应用的外观看上去不错，而对其背后使用的框架产生兴趣，这就像过去一直以来我们挑选音乐的方式。我们会走出门，在商店里看到一张专辑，它可能出自一个你过去听说过的乐队，专辑的封面制作地很不错，上面列出的曲目你也挺感兴趣，于是你就会把它买回来。

也许这张专辑在 Billboard（译者注：美国的音乐排行榜）流行程度排行榜上正排在第一位，也许你只是在那家音乐商店里随意选择听了其中几个曲目，总之你就这样把它买了回来。可是，一旦当你带着你新买的 CD 回到家，用你那强大的、经过高度优化的音响系统来播放它，你就会大失所望。有的人认为，现在人们都是用廉价的头戴式耳机和 MP3 播放器来听音乐，所以即便采样率降低一些、没有低音效果也不会有人在意。而你买回来的专辑正是出自这些人之手。于是你不会感到你现在是在一场音乐会的中心位置聆听美妙的音乐，你只会觉得你正在一间厕所里用手机听这支乐队演奏。这张专辑在它的 look & feel 也就是外观上做足了文章，却忽视了一张音乐专辑所必需的基础架构，即需要能够满足高保真立体声音响系统的播放要求。

> Web 框架并非美丽的圣诞礼物。我们所选用的第一个框架将会长期地影响我们的应用程序。

在这篇文章中，我们将看一看不同的 web 框架是如何处理一些基本问题的。我们会考察它们所支持的环境，与当今标准的符合程度，以及它们的前瞻性和它们在标准之外所做的事情。这篇分析将会帮助我们了解，当我们选择了某种框架时，我们的应用是建立在怎样的基础之上。特别是对企业来说，web 框架并非美丽的圣诞礼物。我们所选用的第一个框架将会长期地影响我们的应用程序。

本文是一个关于 web 框架的系列文章中的一篇。如果你还没有读过我们之前的几篇，你可以从这篇开始阅读：[假如我们像选音乐一样来选择我们的 JavaScript 框架……](https://www.sitepen.com/blog/2017/06/13/if-we-chose-our-javascript-framework-like-we-chose-our-music/)。

### 支持的环境
根据我们应用程序的需要，显然可以得出一个对环境的最小需求集合。可是，即便一个框架可以满足这些需求，去考察一下它为什么要支持这些不同的环境，又是如何做到的，将非常有助于我们了解它在将来会如何支持我们未来的需求。如果这个框架只支持“最新最好的”环境，那么它在将来极有可能也会继续这样做。而如果一个框架试图支持非常过时的环境，那么它就会面临一个问题：在一些较新的浏览器上面，如何去保证框架也能高效率地运行。或者有的框架会在二者之间寻求一个舒服的平衡点？它是不是会只支持一个特定的技术集合以及支持这些技术的浏览器？它是主要针对移动环境还是更适合于桌面环境？它是否对客户端和服务器端都能有效地支持？它是在致力于很好地解决一些相关问题，还是在力求解决所有的问题？如果它试图解决所有的问题，那么会不会在某些地方显得有点死板？

### 与现代标准保持一致
在过去几年里 web 平台发展很快，并且很可能还会继续保持着这样很快的变化速度。去了解一下一个 web 框架如何与当前标准保持一致，可以提示我们它在将来会怎么处理与未来标准的关系。不光要看框架对目前的 ECMAScript 标准和 CSS 标准的支持情况，还要考虑框架是如何处理现代 web 平台中所包含的其它标准的，而且这些标准可能是没有版本编号的、不断变化的“活”标准。

这其实是一柄双刃剑。一些技术在尚未被纳入标准流程的时候就会被有的框架采用。如果一个框架依赖于一些可能永远不会变成官方标准的技术，就要特别警惕。否则，如果他们将来需要与标准重新保持一致，就会破坏框架的向后兼容性，进而导致你的程序出现问题；也有可能标准中最终引入了另外一个同样功能但运行效率更好的实现，这对你来说就演变成了一个性能问题。

![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/laptop-1024x683.jpg)

### 功能增强
而另一方面，如果一个框架一直等着技术标准化之后再去支持它，那它就可能无法提供构建一个现代 web 应用程序所需的完整功能。即便现在的标准演进非常快，web 平台也可能无法提供所有需要的基础功能。上一个时代的 web 平台没有提供足够的高阶功能来让开发者变得更有效率，当时就出现了很多 web 框架来解决这个问题。尽管这个问题在过去几年已经有了很大的改观，但这也并不意味着已经没有什么东西需要被（或者将被）改善。因此作为框架的使用者，我们仍然需要框架帮助我们，而框架在进行这类功能增强时所采用的方式也会对我们产生影响。

### 向前兼容
框架是否考虑过如何保持向前兼容性？也就是说，框架是否能帮助我们这些使用者，避免在 web 平台引入了一些最新功能之后，就不得不重写我们的代码。

### i18n 与 i10n
对于一个 web 框架而言，国际化（i18n）与本地化（i10n）在很多使用场景下都是非常重要的。有许多公司往往认为他们不需要这些功能，但随着互联网的全球化，当他们打算把业务拓展到别的国家时就会发现他们不得不大量重写他们的应用程序。

所谓国际化，就是把像文字这样的资源从一种语言翻译成另外一种语言。而本地化则更为宽泛，不仅仅是翻译文字，还需要处理一些更高阶的概念。不同的文化地域的人们有着不同的数字表达方式，或者说他们怎样对数字进行分割。例如数字1千万在美式英语中表达为 10 million，而在印度则要写成 1 crore。所以对于有这种特定市场需求的应用程序来说，框架所能提供的本地化支持就是非常重要的。

快速跳转：

* [Angular 2+](#angular)
* [React + Redux](#react)
* [Vue.js](#vue)
* [Dojo 2](#dojo)
* [Ember](#ember)
* [Aurelia](#aurelia)

## <span id="angular">Angular 2+</span>
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/angular-logo.png)

### 支持的环境
Angular 2+ 的目标是支持 IE 9 - 11，以及 Firefox、Chrome、Edge、Safari、iOS 和 Android 的现行版本。要达到这一目标，它要求使用者确保在目标环境中要具备必要的 [polyfill](https://angular.io/guide/browser-support)。这些 polyfill 也可以通过一个定制的编译管道纳入到项目的编译过程中。在过去，Angular 曾经迅速放弃过对旧浏览器的支持。

在 IE9 中不支持动画。

所有这些被支持的平台都已经被包含在 Angular 2+ 自己的测试集合中。

### 与现代标准保持一致
Angular 2+ 内部是用 TypeScript 写的，但也支持 ES6 语法。Angular 2+ 也重度依赖一些未来的标准，比如像 [decrators](https://tc39.github.io/proposal-decorators/) 和 [ES observables](https://github.com/tc39/proposal-observable)（通过 RxJS）。Angular 2+ 支持 ES6 模块语法，同时也在此基础上做了一些重要的改动，引入了一些扩展的元数据使一般的模块和类变成 Angular 生态系统的一部分。尽管 Angular 2+ 采用了一种可能是向前兼容的方式来引入这些元数据扩展，但这也意味着使用这些所谓的 NgModules，你将被在某种程度上锁定在 Angular 2+ 的生态系统内。Angular 2+ 采用了 decorators 和 ES observables，但其内部的异步代码还未采用 ```async / await``` 模式。

Angular 2+ 虽然使用了 TypeScript， 但更多的是把 TS 的优势用于写作这个框架本身，TS 中的类型以及其它优点并没有被传递出来用以改善框架使用者的开发体验。Angular 2+ 允许开发人员在 polyfill 的帮助下使用 ES6+ 的语法和其他功能。

### 功能增强
Angular 2+ 中大多数的功能增强都围绕其模板功能以 API 形式提供。虽然这些对于开发会大有帮助，但是也并非必需。使用像 TypeScript 这样的东西有一个优点是可以在开发代码过程中做到智能感知/自动补全以及错误检查。Angular 2+ 则通过语言服务扩展的方式为它的模板开发带来了这些功能，目前至少在 [Visual Studio Code](https://github.com/angular/vscode-ng-language-service) 中已经得到了应用。

Angular 2+ 利用 [core-js](https://github.com/zloirock/core-js) 来进行 ES6 的 polyfill，借助 [RxJS](http://reactivex.io/rxjs/) 实现了一种非常类似于目前还处于建议状态的 ES Observables 语法以及基于此的其它更高级的功能。Angular 2+ 还引入了 **zones** 来为异步代码提供了一个统一的执行上下文。

### 向前兼容性
由于 Angular 2+ 是用 TypeScript 写的，这在一定程度上保证了它将来在句法的向前兼容。同时，Angular 2+ 也依赖于一组标准 —— 它通过 polyfill 保证与标准的一致性。这些都在很大程度上确保了你今天写的代码保持向前兼容。Angular 2+ 也确实重度依赖于它的框架 API，而这些 API 的长期兼容性则取决于 Angular 2+ 的路线图。对此，Angular 2+ 团队表示他们没有破坏未来兼容性的计划，不会再发生从 Angular 1 到 Angular 2 那样的事情了。

Angular 2+ 支持 ES Modules 和组件式架构。通过代码分离，可以更加容易地进行代码重构和测试，避免局部改动对整个代码库的影响。另一方面，因为 Angular 2+ 使用依赖注入，在开发时不依赖于具体的接口和类型。这可能会导致在升级应用程序时发生错误，并且这种错误难以在开发阶段发现。

### i18n 与 i10n
i18n API 是 Angular 2+ 整个框架的一部分。通过向模板增加一个 i18n 属性就可以实现，并且可以搭配一个可选的意图描述。Angular 2+ 的命令行工具会在编译阶段从模板中抽取出这些信息形成用于语言翻译的 bundle，然后你可以为不同的语言地域提供不同的翻译。Angular 2+ 没有为日期、货币、复数以及其他语言结构提供本地化 API。在使用 i18n 时需要选用 [SystemJS](https://github.com/systemjs/systemjs) 这样的加载器插件，它可以动态地加载那些翻译资源。翻译文件的产生依赖于编译过程，因此对翻译的修会迫使你经常重新编译你的程序。因而许多Angular 2+ 的用户已经转向了其它更好的 i18n/i10n 第三方解决方案，例如 [Globalize](https://github.com/jquery/globalize/) 和 [i18next](https://www.i18next.com/)。

## <span id="#react">React + Redux</span>
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/react-logo.png)

### 支持的环境
确切地描述 React + Redux 所支持的浏览器环境是比较复杂的，因为它们二者都是库，可能被用来作为一个大型应用技术栈中的一部分，其中涉及的其它技术所支持的环境与 React + Redux 可能相同也可能不同。[ReactDOM](https://facebook.github.io/react/docs/react-dom.html) 在 React 中负责实现核心的 DOM 交互，它支持 IE9 和其它浏览器，包括 Edge、Safari、Firefox、Chrome、iOS 和 Android 的当前版本。Redux 应该可以在支持 ES5+ 的任何浏览器中工作，所以它支持的环境与 ReactDOM 相同。

同时，React 在服务器端渲染方面也非常成熟，尽管找不到一个确切的它与 Node.js 之间的支持矩阵，但显然它可以支持 Node.js 0.10.0+。

### 与现代标准保持一致
React 和 Redux 都对现代标准持欢迎态度。它们一般都会假定开发者用 ES6+ 来写代码，然后使用一个转译工具来把代码转化成需要的版本。传统上人们都会使用 [Babel](https://babeljs.io/) 作为转译工具，但 [TypeScript](https://typescriptlang.org/) 加上 [core-js](https://github.com/zloirock/core-js) 也可以提供类似的功能。如果你想在不用 [ES6](https://facebook.github.io/react/docs/react-without-es6.html) 的情况下使用 React，尽管你可以找到一些相关资料，但你会很快发现现在这么做已经非常困难了。

React + Redux 也会经常推动未来标准的发展。Facebook 一旦在他们的代码中使用这些标准，就会在标准工作组里面支持它们。例如，React 中就使用了[对象的 spread/rest 操作符](https://github.com/tc39/proposal-object-rest-spread)这一新语法，而类似功能目前还处于 [TC39 的标准化流程](https://www.sitepen.com/blog/2017/04/06/tc39-open-and-incremental-approach-improves-standards-process/)中。

### 功能增强
React 重度依赖 [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)，这是一个可以把 XML 语法融入 JavaScript 的预处理器。尽管不使用 JSX 也可以使用 React，但这也会非常困难，因为社区现在已经接受了 JSX，大多数的例子都是用 JSX 来写的。

React 周边还存在着一批 Facebook 认为提供了重要功能或模式的库。例如 [Immutable.js](https://facebook.github.io/immutable-js/)（这个库提供了不可变对象，来帮助实现单向数据流模式） 和 [Flow](https://flow.org/)（为 JavaScript 提供静态类型支持，帮助实现编译阶段的类型检查）都提供了标准之外的功能。同时 React + Redux 还需要很多其它依赖的库，形成了一套它们所需要的稳定的 API 集合。

React 在运行时还依赖于一些底层库，主要用于确保标准兼容性，以及一些底层的环境侦测。Redux 则使用 [Lodash](http://lodash.com/) 来提供一些非标准的高级功能。

### 向前兼容性
由于 React + Redux 鼓励使用 ES6+ 语法，因此它们应当具备良好的向前兼容性。它们也鼓励开发者使用 ES 模块来提高代码的可维护性，并且 React 本身也是以组件式架构来构建的。但是，由于我们不可能只用 React + Redux 就搭建出整个应用程序，我们势必会在程序中使用其它的库，这会增加在向前兼容性方面的风险，可能需要我们更频繁地去重构代码以保持与社区的最佳实践保持一致。

### i18n 与 i10n
React 或 Redux 内部都没有直接支持 i18n 和 i10n。

最常见的处理 i18n 和 i10n 的方式是借助来自 Yahoo! 的项目 [format.js](https://formatjs.io/)（它是从 [messageformat.js](https://messageformat.github.io/) 中派生出来的，messageformat.js 提供了对 React、Ember 和 Handlebars 的整合，Handlebars 则是 Ember 的模板引擎）。format.js 提供了一组健的 API来处理语言翻译，也可以解决了很多应用程序面临的本地化问题。

## <span id="#vue">Vue.js</span>
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/vue-logo.png)

### 支持的环境
Vue.js 目标支持 IE9 - 11 和 Firefox、Chrome、Edge、 Safari、iOS 以及 Android 的当前版本。部分功能需要加载 polyfills/shims。

Vue.js 的完整编译发布版本由于在其模板编译器中使用了动态 JavaScript 功能，无法工作于 CSP（Content Security Protocol）环境。但其运行时版本因为把模板预编译成了 JavaScript 函数，则可以用于 CSP 环境。

Vue.js 也可以在纯 Node.js 环境中运行，从而做到服务器端渲染（SSR）。Vue.js 的 SSR 应可以工作于 Node.js 0.10.0+。

### 与现代标准保持一致
Vue.js 2 是用 ES6+ 语法和 ES 模块写的，借助 Babel 来使其可以在老式浏览器中运行，同时它利用了 webpack 进行 ES 模块的编译时打包。

Vue.js 2 使用 [Flow](https://flow.org/) 来实现静态类型检查，对于使用 Vue.js 的开发者而言，无论使用 Flow、TypeScript 还是纯原生的 JavaScript/ES6 都会非常方便。

Vue.js 的结构和语法都深受 Web Components 理念的影响，但它并没有直接使用 Web Components 或者相关的技术，比如像 shadow DOM 或者 CSS 作用域。Vue.js 通过它的“单文件组件”来提供类似的功能。顾名思义，所谓“单文件组件”就是一个 Vue.js 的组件可以被完整地定义在一个单独的文件里，其中将会包含一个 HTML 模板还有 JavaScript 和 CSS。这种组件必须使用 WebPack 及其特定插件来编译，只有这种插件才可以理解 .vue 文件中的组件语法。作为编译过程的一部分，CSS 会被抽取出来并赋以作用域。

### 功能增强
Vue.js 的 API 相当少，除了渲染和管理组件所必需的之外，没有再提供额外的工具函数。

虽然 Vue.js 在编译阶段会用到 [Weex](https://weex.apache.org/)、lodash、Babel polyfills 和各种底层 API 进行协作，但它在运行时却没有任何直接的外部依赖。

### 向前兼容性
Vue.js 可以运行在任何支持 ES5 的浏览器上，在可以预见的未来几乎所有的浏览器都能做到这一点。如果是在脚本或者 HTML 页面中直接写一个 Vue 组件，你可以使用当前环境支持的任意版本的JavaScript。而单文件组件则需要配置 WebPack 进行编译，通过在编译工具链中整合 Babel 可以使其默认支持 ES6+ 语法。

尽管 Web Components 标准在浏览器端的支持还比较少，进展也比较缓慢，但 Vue.js 还是做了巨大的努力来与这一标准保持一致。现在很难预测 Web Components 最终是否会成为一套足够稳定的标准，但如果在将来真有这么一天，Vue.js 无疑会去采用这一标准。这可能意味着到时你需要重构你的组件，但 Vue.js 在方便用户进行版本升级方面一向做得很好。

### i18n 与 i10n
Vue.js 没有内建对 i18n 和 i10n 的支持，但有很多第三方库可以做这件事。特别像 [Awesome Vue](https://github.com/vuejs/awesome-vue#i18n) 目前是被用得最多的。

## <span id="#dojo">Dojo 2</span>
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/dojo-logo.png)

### 支持的环境
Dojo 2 的目标及测试环境是 IE 11、Android 4.4+、iOS 9.1+ 以及 Chrome、Safari、Edge 和 Firefox 的当前版本。

Dojo 2 的路线图中还包括在 Node.js 6+ 环境中进行服务器端渲染，目前与之相关的少部分功能也已经完成。

### 与现代标准保持一致
Dojo 2 是用 TypeScript 写的，全面采用了 ES6+ 语法。在支持 ES6+ 功能方面，Dojo 2 没有采用全局的 polyfills，而是使用了 shim 模块。这样不会污染到 global 作用域，如果宿主环境支持某项 ES6+ 功能，它就会转用浏览器的原生实现。除了 ES Observables 还未被最终确定是否纳入 ECMAScript 中，Dojo 2 所提供的所有 shim 模块都对应着已发布的标准。

Dojo 2 提倡使用 TypeScript 所支持的类和方法装饰器语法，同时也支持不用装饰器语法来实现一样的功能。CSS 方面则使用 CSS 3+ 语法中的 CSS 模块形式，在编译阶段，它们会被转写成具有名字空间的、低版本的 CSS。

Dojo 2 会较早地采用一些与其设计目标一致的标准。比如像指针事件 PointerEvents、自定义元素 CustomElements、Intersection Observers、Web 动画、```async/await```、动态```import()```和对象的 rest/spread 操作符。

Dojo 2 提供的组件可以被导出为 Web Components，进而可以在其它框架中**独立**使用，这时这些组件只会向外暴露与 Web Components 标准一致的 API。

### 功能增强
在上述这些标准之上的功能增强都位于```@dojo/core```内，在这里提供了一些 Dojo 2 认为有用的一些高阶功能，包括一些抽象实现，例如可中止的 promise、请求资源等。

Dojo 2 在运行时对 [Globalize](https://github.com/globalizejs/globalize) 和 [Maquette](http://maquettejs.org/) 有外部依赖，同时需要一些对标准的 polyfills（[pep.js](https://github.com/jquery/PEP) 和 [Intersection Observers](https://github.com/WICG/IntersectionObserver/tree/gh-pages/polyfill)）。

### 向前兼容性
Dojo 2 用 TypeScript 编写保证了在句法上的向前兼容性。它同时也极力鼓励基于 Dojo 2 开发的应用程序也使用 TypeScript，这样就可以带来同等的向前兼容。除了 polyfills 和 shims，TypeScript 也对 Dojo 2 的向后兼容性有很大的帮助。

尽管在 AMD 环境中也可以使用 Dojo 2，但它强烈建议开发者通过 ```dojo build```命令行工具使用 Webpack 2 来编译应用程序，这样可以避免未来在打包和模块加载方面的变化对你的代码及其结构产生影响。

通过采用 ES Modules 和 CSS Modules，Dojo 2 提倡让每个组件描述自己的全部依赖。借助于在代码和 CSS 中的强制类型化，Dojo 2 试图建立一个可以在开发/编译阶段而不是运行的时候就能发现问题的系统。这样即便框架底层的改变会影响其上的应用程序，问题也可以被很容易地被发现。

Dojo 2 打破了 Dojo 1 已经维持了超过 10 年的向前兼容。希望其后续版本不要再发生这样破坏兼容性的事情，而以不断的小规模迭代的方式来进行版本更新。

### i18n 与 i10n
对 i18n 和 i10n 的支持已经内建于 Dojo 2 的组件系统。利用 Globalize.js 和官方的 [Unicode CLDR](http://cldr.unicode.org/) 数据，Dojo 2 应用程序可以侦测和改变当前使用的语言环境，开发者可以在组件中描述翻译的文字和其它需要本地化的信息。

Dojo 2 的命令行编译工具可以直接把语言翻译信息打包进模块，或者让它们在运行时被动态加载。

## Ember
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/ember-logo.png)

### 支持的环境
Ember 的支持目标是 IE 9 - 11 和 Firefox、Chrome、Edge、Safari、iOS 及 Android 的当前版本。

Ember 也提供了一种名为 **FastBoot** 的模式来进行服务器端渲染。这一模式支持 Node.js 4+。这里需要注意的是，如果引入了一些 Ember 的 add-on，其带来的外部依赖可能与 FastBoot 环境不兼容，有可能导致服务器端渲染无法工作。

### 与现代标准保持一致
尽管其渲染引擎 Glimmer 是用 TypeScript 和 ES6+ 语法写的，但 Ember 可以支持 ES5。很多 Ember 的使用者都开始采用现代语法，并使用转写工具来保证向后兼容性。

### 功能增强
从 Ember 2 开始，Ember 不再在内部直接使用 jQuery 来提供功能增强。默认情况下，jQuery 仍被包含在 Ember 内，但 Ember 提供了一套代理 API 来使用 jQuery。通过这种方式，如果你愿意，你可以完全去除对 jQuery 的依赖。Ember 把 Babel 引入其编译过程，来提供对标准的 polyfills。

### 向前兼容性
Ember 对未来的兼容性做了充分的考虑，甚至提供了一条非常可靠的升级路径来帮助用户从 1.13 升级到 2.0。

Ember 团队已经表示他们正在设法分拆他们的的平台，使之更加模块化，把各种功能分散到不同的包里。他们之所以这样做，是因为他们感到 Ember 一直被人诟病过于庞大。

### i18n 与 i10n
Ember 没有提供内建的 i18n 功能，但存在一些第三方的解决方案，例如 [ember-i18n](https://github.com/jamesarosen/ember-i18n) 和 Yahoo! 的 [ember-intl](https://github.com/ember-intl/ember-intl)（它同时也是 [Format.js](https://formatjs.io/) 的组成部分）。

## Aurelia
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/aurelia-logo.png)

### 支持的环境
Aurelia 把精力集中于现代浏览器的当前版本，如：Microsoft Edge、Chrome、Firefox 和 Safari。尽管它也想要支持像 IE 11 这样的浏览器，但似乎出现在这些平台上的问题并不会被迅速解决。

Aurelia UX 对 web 有着与 Cordova 和 Electron 相同的理念，尽管 Cordova 和 Electron 目前也都还处于发展之中。Aurelia 也有服务器端渲染的目标，但还未实现。

### 与现代标准保持一致
Aurelia 用 ES6+ 开发，并使用 Babel 来把源代码转写成需要的版本。Aurelia 使用它们自己的 polyfills 包来提供向后兼容性和一些未来标准中的功能。Aurelia 内部并不直接处理 CSS 3+，但可以很容易地整合一些 CSS 预处理器和像 [postcss](http://postcss.org/) 这样的后处理器。

Aurelia UX 则是用 TypeScript 开发。 Aurelia 为其框架提供了一组完整的类型系统，可以支持开发者使用 TypeScript 进行应用开发。

### 功能增强
除了在一些老的浏览器中运行时需要 Babel 来提供一些功能上的 polyfills，Aurelia 没有其它外部依赖。Aurelia 自行实现了它所需要的基础功能增强，并把它们分散到不同的包里，形成所谓 Aurelia umbrella 的一部分。这包括：数据绑定、模板与自定义元素、依赖注入、路由、样式作用域、主题和平台侦测。这些功能有的被包含在框架的默认包里，其它的则可以以独立包的方式提供。

### 向前兼容性
Aurelia 的目标是可以与[未来两三年的 web 平台](https://github.com/aurelia/framework/blob/master/doc/article/en-US/technical-benefits.md#modern-javascript)保持兼容。它支持使用 TypeScript 进行下游应用开发，这一点可能会使这些下游应用更容易实现向前兼容性。Aurelia使用 Babel 转写 ES6+ 源代码，使用现代 DOM API 并借助 polyfills 可以实现在 IE 9 中运行，尽管 IE 9 并不在官方的支持列表中。在内部，Aurelia 采用了 Web Components 标准，目标是支持导出 UX 组件为 Web Components，但这一目标目前还未实现。

### i18n 与 i10n
Aurelia 提供了一个 [i18n 库](https://github.com/aurelia/i18n)，其内部其实是用 [i18next](https://www.i18next.com/) 实现。i18next 不仅提供了处理翻译的能力，也提供了其它 i10n 功能，比如对复数的处理，还提供了一套健壮的 API 来实现像数字格式化这样的高级本地化功能。

## 总结
### Angular 2+
尽管 Angular 2+ 依赖于一些别的项目，这些项目也不受 Angular 的直接控制，而且有的规模还不小，但 Angular 2+ 仍然可以说是建立在一个相当不错的基础之上。使用转写工具，保证了一定程度上的向前兼容；但使用其元数据扩展则会导致对你在某种程度上锁定在  Angular 上，你将必须一直与他们的架构保持一致。

### React + Redux
React 和 Redux 都是灵活的工具和库。从它们自身而言，他们并没有提供一个完整的应用程序开发解决方案。你会很轻易地在你的应用程序中堆砌很多额外的库来增加其它的功能，然后会发现你的应用已经变得臃肿不堪而且充满各种问题。你当然可以为你的应用程序建立一个有效且高效的基础架构，但这确实需要你具备丰富的经验和足够的技术。而且 React 和 Redux 都有大量的外部依赖作为它们的基础，它们会很容易地向你的应用中堆积更多的“垃圾”。如果你确信你真的想“自己卷烟卷“，那么 React 和 Redux 确实是非常优秀的库。但它们并不是可以开箱即用的框架。

### Vue.js
如果你热衷于 MVVM 应用模型，那么你很难不去考虑一下 Vue.js。Vue.js 提供一组现代 API 作为你应用开发的坚实基础， 它将上游依赖融入到了一组完整的开发者 API 之中。

### Dojo 2
Dojo 2 力图提供一个良好的基础，并试图最小化它所需要的依赖。Dojo 2 采用 TypeScript 和现代的语法及功能给予它相当大的发展空间，无需将来再次重新架构。Dojo 2 仍然处于 beta 阶段，对你的应用而言，这可能意味着存在着基础上的不稳定性。但如果你在将来打算用 TypeScript 开发， Dojo 2 真的是唯一有效的选择。

### Ember.js
Ember 拥有一个健壮的生态系统，而且它也是真的下功夫在维持这个生态系统，让人们在这个基础之上可以更容易地开发，进而建立一个更大的生态系统。Ember 所提供的基础可能不像一些别的框架那么全面，因此你想要的功能可能需要通过 add-on 来提供。尽管 Ember 的生态圈似乎很庞大且自律，但这些 add-on 的质量还是可能会参差不齐。

### Aurelia
如果你喜欢依赖注入的概念，喜欢一个模板导向的框架，还希望能够与当前和未来的标准保持一致，那么 Aurelia 可能就是你想要的那个框架。尽管 Aurelia 的基础目前已经接近完备，但仍然有些组成部分尚未完成，不过， Aurelia 的方向与发展目标是非常清晰的，其整体架构也是可持续的。

# 下节预告
我们已经看过了专辑唱片的封面，阅读了其上的文字，知道了这里面的音乐是如何制作的，接下来我们真正要做的就是我们拿到一张专辑后都肯定会去做的一件事：坐下来，聆听这张唱片。这就是下一篇我们要讨论的内容。我们使用这些 web 框架的目的是要创建 web 应用程序，接下来的这篇文章我们会探讨一下，不同的框架是如何处理“应用程序”这个概念的。

***

本系列的其它文章：

1. [假如我们像选音乐一样来选择我们的 JavaScript 框架……](https://www.sitepen.com/blog/2017/06/13/if-we-chose-our-javascript-framework-like-we-chose-our-music/)
2. [Web 框架：用户界面开发](https://www.sitepen.com/blog/2017/06/16/web-frameworks-user-interface-development/)
3. [Web 框架：用户体验设计](https://www.sitepen.com/blog/2017/06/27/web-frameworks-user-experience-design/)
4. Web 框架：基础技术
5. [Web 框架：应用](https://www.sitepen.com/blog/2017/08/03/web-frameworks-applications/)