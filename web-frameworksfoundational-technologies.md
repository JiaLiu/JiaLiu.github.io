# Web 框架：基础技术

> 本文译自：[Web Frameworks: 
Foundational Technologies](https://www.sitepen.com/blog/2017/07/06/web-frameworksfoundational-technologies/)  
作者：[Kit Kelly](https://www.sitepen.com/blog/author/kkelly/)

![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/blog-7.jpg)

在之前的文章离我们讨论了一些 web 框架的 “look & feel“，也就是它们如何解决用户界面开发与样式定义的问题。我们经常因为一些组件和应用的外观看上去不错，而对其背后使用的框架产生兴趣，就像过去一直以来我们挑选音乐的方式。我们会出门，然后看到一张专辑，它可能出自一个你过去听说过的乐队，专辑的封面制作地很不错，上面列出的曲目你也挺感兴趣，于是你就会把它买回来。

也许这张专辑在 Billboard（译者注：美国的音乐排行榜）流行程度排行榜上正排在第一位，也许你只是在那家音乐商店里随意选择听了其中几个曲目，总之你就这样把它买了回来。可是，一旦当你带着你新买的 CD 回到家，用你那强大的、经过高度优化的音响系统来播放它，你就会大失所望。有的人认为，现在人们都是用廉价的头戴耳机和 MP3 播放器来听音乐，所以即便采样率降低一些、没有低音效果也不会有人在意。而你买回来的专辑正是出自这些人之手。于是你不会感到你现在是在一场音乐会的中心位置聆听美妙的音乐，你只会觉得你正在一间厕所里用手机听这支乐队演奏。这张专辑在它的 look & feel 也就是外观上做足了文章，却忽视了一张音乐专辑所必需的基础架构，即需要能够满足高保真立体声音响系统的播放要求。

> Web 框架并非美丽的圣诞礼物。我们所选用的第一个框架将会长期地影响我们的应用程序。

在这篇文章中，我们将看一看不同的 web 框架是如何处理一些基本问题的。我们会考察它们所支持的环境，与当今标准的符合程度，以及它们的前瞻性和它们在标准之外所做的事情。这篇分析将会帮助我们了解，当我们选择了某种框架时，我们的应用是建立在怎样的基础之上。特别是对企业来说，web 框架并非美丽的圣诞礼物。我们所选用的第一个框架将会长期地影响我们的应用程序。

本文是一个关于 web 框架的系列文章中的一篇。如果你还没有读过我们之前的几篇，你可以从这篇开始阅读：[假如我们像选音乐一样来选择我们的 JavaScript 框架……](https://www.sitepen.com/blog/2017/06/13/if-we-chose-our-javascript-framework-like-we-chose-our-music/)。

### 支持的环境
根据我们应用程序的需要，显然可以得出一个对环境的最小需求集合。可是，即便一个框架可以满足这些需求，去考察一下它为什么要支持这些不同的环境，又是如何做到的，将非常有助于我们了解它在将来会如何支持我们未来的需求。如果这个框架只支持“最新最好的”环境，那么它在将来极有可能也会继续这样做。而如果一个框架试图支持非常过时的环境，那么它就会面临一个问题：在一些较新的浏览器上面，如何去保证框架也能高效率地运行。或者有的框架会在二者之间寻求一个舒服的平衡点？它是不是会只支持一个特定的技术集合以及支持这些技术的浏览器？它是主要针对移动环境还是更适合于桌面环境？它是否对客户端和服务器端都能有效地支持？它是在致力于很好地解决一些相关问题，还是在力求解决所有的问题，但也许在某些领域会显得多少有点刻板？

### 与现代标准保持一致
在过去几年 web 平台的发展很快，并且很可能还会继续保持一个很快的变化速度。去了解一下一个 web 框架是如何与当前标准白吃一致，可以提示我们在这个问题上它在将来会怎么做。不光要看框架对目前的 ECMAScript 标准和 CSS 标准的支持情况，还要考虑框架是如何处理现代 web 平台中所包含的其它标准的，而且这些标准可能是没有版本编号的、不断变化的“活”标准。

这其实是一柄双刃剑。一些技术在尚未被纳入标准流程的时候就会被有的框架采用。如果一个框架依赖于一些可能永远不会变成官方标准的技术，就要特别警惕。否则，如果他们将来需要与标准重新保持一致，就会破坏框架的向后兼容性，进而导致你程序中的问题；也有可能标准中最终引入了另外一个同样功能但运行效率更好的实现，这对你来说就演变成了一个性能问题。

### 功能增强
而另一方面，如果一个框架一直等着技术标准化之后再去支持它，那它就可能无法提供构建一个现代 web 应用程序所需的完整功能。即便现在的标准演进非常快，web 平台也可能无法提供所有需要的基础功能。而这正是过去那些框架所主要解决的问题，那时的 web 平台没有提供足够的高阶功能来让开发者变得更有效率。这个问题在过去几年已经有了很大的改观，但这也并不意味着已经没有什么东西需要被（或者将被）改善。因此框架在进行这类功能增强时所采用的方式也会对我们这些框架的使用者产生影响。

### 向前兼容
框架是否考虑过如何保持向前兼容性？也就是说，框架如果能帮助我们这些使用者，不需要在 web 平台引入了一些最新功能之后，就不得不每六个月就重写我们的代码。

### i18n 与 i10n
对于一个 web 框架而言，国际化（i18n）与本地化（i10n）在很多使用场景下都是非常重要的。有许多公司往往认为他们不需要这些功能，但随着互联网的全球化，当他们打算把业务拓展到别的国家时就会发现他们不得不大量重写他们的应用程序。

所谓国际化，就是把像文字这样的资源从一种语言翻译成另外一种语言。而本地化则更为宽泛，不仅仅是翻译文字，还需要处理一些更高阶的概念。不同的文化地域的人们有着不同的数字表达方式，或者说他们怎样对数字进行分割。例如数字1千万在美式英语中表达为 10 million，而在印度则要写成 1 crore. 所以对于有这种特定市场需求的应用程序来说，框架所能提供的本地化支持可能就是非常重要的。

## Angular 2+
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/angular-logo.png)

### 支持的环境
Angular 2+ 的目标是支持 IE 9 - 11，以及 Firefox、Chrome、Edge、Safari、iOS 和 Android 的现行版本。要达到这一目标，它要求使用者确保在目标环境中要具备必要的 [polyfill](https://angular.io/guide/browser-support)。这些 polyfill 也可以通过一个定制的编译管道纳入到项目的编译过程中。在过去，Angular 曾经迅速放弃过对旧浏览器的支持。

在 IE9 中不支持动画。

所有这些被支持支持的平台都已经被包含在 Angular 2+ 自己的测试集合中。

### 与现代标准保持一致
Angular 2+ 内部是用 TypeScript 写的，但也支持 ES6 语法。Angular 2+ 也重度依赖一些未来的标准，比如像 [decrators](https://tc39.github.io/proposal-decorators/) 和 [ES observables](https://github.com/tc39/proposal-observable)（通过 RxJS）。Angular 2+ 支持 ES6 模块语法，同时也在此基础上做了一些重要的改动，引入了元数据来使这些模块和类成为 Angular 生态系统的一部分。这些额外引入的元数据，尽管采用了一种可能是向前兼容的方式来添加，但这也意味着使用它们，你将被在某种程度上锁定在 Angular 2+ 的生态系统内。这样的模块被称为 NgModules。虽然 Angular 2+ 采用了 decorators 和 ES observables，但像 async / await 这样的东西还尚未用到其内部的异步代码上。

Angular 2+ 虽然使用了 TypeScript， 但更多的是把 TS 的优势用于写作这个框架本身，例如 TS 中的类型以及其它优点并没有被改善框架使用者的开发体验。Angular 2+ 仍然允许开发人员在 polyfill 的帮助下使用 ES6+ 的语法和其他功能。

### 功能增强
Angular 2+ 中大多数的功能增强都围绕其模板功能以 API 形式提供。虽然这些对于开发会大有帮助，但是也并非必需。使用像 TypeScript 这样的东西有一个优点是可以在开发代码过程中做到智能感知/自动补全以及错误检查。Angular 2+ 则通过语言服务扩展的方式为它的模板开发带来了这些功能，目前至少在 [Visual Studio Code](https://github.com/angular/vscode-ng-language-service) 中已经得到了应用。

Angular 2+ 利用 [core-js](https://github.com/zloirock/core-js) 来进行 ES6 的 polyfill，借助 [RxJS](http://reactivex.io/rxjs/) 实现了一种非常类似于目前还处于建议状态的 ES Observables 语法以及基于此的其它更高级的功能。Angular 2+ 还引入了 zones 来为异步代码提供了一个统一的执行上下文。

### 向前兼容性
由于 Angular 2+ 是用 TypeScript 写的，这在一定程度上保证了它将来在句法上不会发生巨大的变化。同时，Angular 2+ 也依赖于一组标准 —— 通过 polyfill 保证与标准的一致性。这些都在很大程度上确保了你今天写的代码保持向前兼容。Angular 2+ 也确实重度依赖于它的框架 API，而这些 API 的长期兼容性则取决于 Angular 2+ 的路线图。对此，Angular 2+ 团队表示他们没有破坏未来兼容性的计划，不会再发生从 Angular 1 到 Angular 2 那样的事情了。

Angular 2+ 支持 ES Modules 和组件式建构。通过代码分离，可以更加容易地进行代码重构和测试，而避免对整个代码库的影响。另一方面，因为 Angular 2+ 使用依赖注入，在开发时不依赖于具体的接口和类型。这可能会导致在升级应用程序时发生错误，并且这种错误难以在开发阶段发现。

### i18n 与 i10n
i18n API 是 Angular 2+ 整个框架的一部分。通过向模板增加一个 i18n 属性就可以实现，并且可以搭配一个可选的意图描述。Angular 2+ 的命令行工具会在编译阶段从模板中抽取出这些信息形成用于语言翻译的 bundle，然后你可以为不同的语言地域提供不同的翻译。Angular 2+ 没有为日期、货币、复数以及其他语言结构提供本地化 API。在使用 i18n 时需要选用 [SystemJS](https://github.com/systemjs/systemjs) 这样的加载器插件，它可以动态地加载那些翻译资源。这些翻译文件的产生依赖于编译过程，因此扩展它们将经常需要你重新编译你的程序。因而许多Angular 2+ 的用户已经转向其它更好的 i18n/i10n 第三方解决方案，例如 [Globalize](https://github.com/jquery/globalize/) 和 [i18next](https://www.i18next.com/)。

## React + Redux
![](https://www.sitepen.com/blog/wp-content/uploads/2017/06/react-logo.png)

### 支持的环境
确切地描述 React + Redux 所支持的浏览器环境是比较复杂的，因为它们二者都是库，可能被用来作为一个大型应用技术栈中的一部分，其中涉及的其它技术所支持的环境与 React + Redux 可能相同也可能不同。[ReactDOM](https://facebook.github.io/react/docs/react-dom.html) 在React 中负责实现核心的 DOM 交互，它支持 IE9 和其它浏览器，包括 Edge、Safari、Firefox、Chrome、iOS 和 Android 的当前版本。Redux 应该可以在支持 ES5+ 的任何浏览器中工作，所以它支持的环境与 ReactDOM 相同。

同时，React 在服务器端渲染方面也非常成熟，尽管找不到一个确切的它与 Node.js 之间的支持菊展，但显然它可以支持 Node.js 0.10.0+。

### 与现代标准保持一致
React 和 Redux 都对现代标准持欢迎态度。它们一般都会假定开发者会用 ES6+ 来写代码，然后使用一个转译工具来把代码转化成需要的版本。传统上人们都会使用 [Babel](https://babeljs.io/) 作为转译工具，但 [TypeScript](https://typescriptlang.org/) 加上 [core-js](https://github.com/zloirock/core-js) 也可以提供类似的功能。对于如何在没有 [ES6](https://facebook.github.io/react/docs/react-without-es6.html) 的支持下使用 React，尽管有一些指导资料，但你会很快发现这已经越来越难了。

React + Redux 也会经常推动未来的标准。Facebook 会在他们的代码中使用这些标准，然后就会在标准工作组里面支持它们。例如，React 中采用了[对象的 spread/rest 操作符](https://github.com/tc39/proposal-object-rest-spread)这一新语法，而类似功能目前还处于 [TC39 的标准化流程](https://www.sitepen.com/blog/2017/04/06/tc39-open-and-incremental-approach-improves-standards-process/)中。

### 功能增强
React 重度依赖 [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)，这是一个可以把 XML 语法融入 JavaScript 的预处理器环节。尽管不使用 JSX 也可以使用 React，但这也会非常坤嫩，因为社区现在已经接受了 JSX，大多数的例子都是用 JSX 来写的。

React 周边还存在着一批 Facebook 认为提供了重要功能或模式的库。例如 [Immutable.js](https://facebook.github.io/immutable-js/)（这个库提供了不可变对象，来帮助实现单向数据流模式） 和 [Flow](https://flow.org/)（为 JavaScript 提供静态类型支持，帮助实现编译阶段的类型检查）都提供了标准之外的功能。同时 React + Redux 还需要很多其它依赖的库，形成了一套它们所需要的稳定的 API 集合。

React 在运行时还依赖于一些底层库，主要用于确保标准兼容性，以及一些底层的环境侦测。Redux 则使用 [Lodash](http://lodash.com/) 来提供一些非标准的高级功能。