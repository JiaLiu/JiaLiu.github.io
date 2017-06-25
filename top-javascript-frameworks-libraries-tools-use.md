# 2017年最佳 JavaScript 框架、库及工具

> 本文译自 [Best JavaScript Frameworks, Libraries and Tools to use in 2017](https://www.sitepoint.com/top-javascript-frameworks-libraries-tools-use/#toolsgeneralpurposetaskrunners)。

似乎现在 JavaScript 框架、库和工具的数量比开发者的数量还要多。截至2017年5月，通过[在 GitHub 上的快速搜索](https://github.com/search?utf8=%E2%9C%93&q=language:JavaScript&type=Repositories&ref=advsearch&l=JavaScript&l=)可以找到超过110万个 JavaScript 项目。在 [npmjs.org 上现在有50万个有用的包](https://www.npmjs.com/)，每个月的下载量几乎达到100亿次。

> 文章内容更新于2017年5月29日，只反映当时 JavaScript 生态圈的状态。

这篇文章试图简单地比较一下当前最流行的 JavaScript 客户端框架、库和工具之间的不同之处。至于它们对你来说是否是“最好的”，这就是另一个问题了。你需要做出一些选择并坚持使用它们一段时间才能做出回答。但请记住，无论你选择了什么，它们都会被将来出现的一些“更好”的东西所取代。

在阅读这篇文章之前请同意下列条款……！
* JavaScript 业内每天都在发生变化。这篇文章在它发布的时候就已经过时了！
* 所谓“最佳”，我的意思是指“最流行的通用性项目”。他们都是自由或者开源软件项目，但是其中可能并未包含你个人喜欢的一些项目。
* 本文不会包含一些已经终止开发的项目，即便它们可能仍然在 Web 上被大量使用，例如 [YUI](https://yuilibrary.com/)。
* 我们仅涉及客户端项目。其中有些项目可以同时在服务器端工作，但像 [Express.js](https://expressjs.com/) 和 [Hapi](https://hapijs.com/) 这样的的纯服务器端项目不在讨论之列。
* 文章对每个项目的信息仅进行简略的介绍和概览，有兴趣的读者可以自行作深入了解。
* 对于每个项目，文中都给出了一个使用率的指标，但众所周知，这种统计数据是很难被证实的，因此可能会有一些误导。
* 我们每个人都是有偏见的。我并没有尝试过这里列出的每样工具，仅仅是列出了我所喜欢的一些项目。你应该基于你的需求做出你自己的评估。
* 如果你基于这片文章做出了极端错误的决定，我和 SitePoint (译者注：发表本文原文的网站)都不会对此负责！
## 棘手的术语定义
在不同的上下文环境中，“框架”、“库”和“工具”这三个词对于不同的人在不同的时间可能意味着不同的东西。 在这里它们的定义如下：

### 库
**库**是一些实用性功能的有组织的集合。一个典型的库可能由一些函数来组成，它们可被用于处理字符串、日期、HTML DOM 元素、事件、cookie、动画、网络请求等等东西。应用程序会调用这些函数，与你选择什么框架来构建应用程序无关，库函数都只是把结果返回给调用它的应用程序。你可以把它们想象成一组汽车零件：你可以自由选择任意的零件来帮助构建一辆可以行驶的车辆，但是驱使这些零件的引擎必须由你自己来建造。

库一般都会提供一个高阶抽象来隐藏实现的细节和消除不一致性。例如当我们利用 [XMLHttpRequestAPI](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) 来实现 Ajax 时总是要写几行代码，而且在不同浏览器之间写法还会有些许的不同。这时一个库就可以提供一个简单的 `ajax()` 函数来解决这个问题，而你就可以把精力集中在实现更高层的业务逻辑上。

当使用库时你不必再关注一些实现上的支端末节，它可以帮助你节省20%的开发时间。但它也有下列的一些缺点：
* 当程序错误是由库内部的 bug 导致的，你很难发现和修正错误
* 库的开发团队并不能保证能及时发布一个补丁来修正他们的错误
* 库的补丁可能会改变原有的 API 进而导致你的代码也必须做出重大的改动

### 框架
**框架**是一个应用程序的骨架。它要求你遵循一定的方式进行软件设计，并在特定的位置插入你自己的逻辑。框架一般都会提供诸如事件、存储和数据绑定这样的功能。仍然用汽车来做一个类比，框架相当于提供了可以工作的底盘、车身和引擎。而你在不破坏汽车功能的前提下可以添加、去除或者修改一些组成部件。

框架一般会提供比库更高阶的抽象，帮助你快速搭建项目起初80%的部分。它的缺点则是：
* 如果你的应用需求超出了框架能提供的范畴，那剩余20%的工作将会是个很大的挑战
* 尽管并非不可能，但升级一个框架是非常困嫩的
* 框架的核心代码与概念很难与时俱进。而随着时间推移，开发者总是能发现更好的方式来做同样的事情，老的框架就会过时

### 工具
**工具**的作用在于辅助开发，它并不是你项目的组成部分。例如构建工具、编译器、转译器、代码压缩工具、图片压缩工具、部署机制等等。

工具应该使开发流程变得更简易。比如许多开发者更喜欢 [Sass](http://sass-lang.com/) 而不是 CSS，因为前者可以提供代码分离、嵌套、渲染时变量、循环和函数等功能。但浏览器并不能理解 Sass/SCSS 语法，因此在测试和部署之前，必须用一个适当的工具来把 Sass 代码编译成 CSS 代码。
### 不要给我打标签！
库、框架和工具之间的区别其实并不那么清晰。一个框架中可能也包含一个库，而一个库也可以实现像框架一样的方法。而构建一个库或者框架则都离不开工具的帮助。在后文中我试图标明每个项目到底是库、框架还是工具，但分类时口径可能并不那么一致。
  
如果这些听起来都太复杂了，你也可以考虑使用**原生 Javascript**。这没什么不好的，但最终你肯定会创造出自己的库或者框架级别的代码，它们也同样像别的框架和库一样需要你长期维护。而其实 JavaScript 自身也是建立于浏览器和操作系统抽象之上的一个高阶抽象。
## JavaScript 框架和库
下列项目按照流行程度排序……
### jQuery
|jQuery||
|----|----|
|类型|库|
|网站|[jquery.com](http://jquery.com/)|
|代码库|[github.com/jquery/jquery](https://github.com/jquery/jquery)|
|当前版本|3.2.1|
|开发者|jQuery 团队|
|发布日期|2006年8月|
|体积|压缩后 30kb|
|用于|通用于各种项目|
|使用率|[72.4%的网站](https://w3techs.com/technologies/details/js-jquery/all/all)|

jQuery一直是有史以来使用最多的 JavaScript 库，它还被包含在 WordPress、ASP.NET 和几个其它框架中一同分发。jQuery 引入了 CSS 选择器进行 DOM 节点的获取，并可以通过链式调用挂载事件处理器、执行动画以及 Ajax 操作，这些都对客户端开发产生了革命性的影响。

近几年喜爱 jQuery 的人已经有所减少，但它对于一些并不需要太多 JavaScript 功能的项目来说仍是一个切实可行的选项。

优点：
* 分发包的体积较小
* 学习难度不高，网上可找到大量的帮助信息
* 语法简洁
* 容易扩展

缺点：
* 与使用原生 API 相比有性能损失
* 现在浏览器的兼容性问题已经得到了改善，所以它在这方面的价值显得不那么重要了
* 使用率不再增长了
* [业界](https://www.sitepoint.com/do-you-really-need-jquery/?aref=cbuckler)已经出现了一些[反对](http://youmightnotneedjquery.com/)滥用 jQuery 的意见

### React
|React||
|----|----|
|类型|库|
|网站|[facebook.github.io/react/](https://facebook.github.io/react/)|
|代码库|[github.com/facebook/react](https://github.com/facebook/react)|
|当前版本|15.5.4|
|开发者|Facebook 及其他贡献者|
|发布日期|2013年3月|
|体积|压缩后 21kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-react/all/all)|

这可能是过去一年中被讨论最多的库了。React 宣称自己是一个用于创建用户界面的 JavaScript 库。它主要关注模型-视图-控制器（MVC）开发中的“视图”部分，使用 React 可以更为容易地创建具有状态的用户界面组件。它首创性地实现了虚拟 DOM，这是一种内存中的数据结构可以通过比较结构的变化来更有效率地更新页面。

React 的使用率统计数据较低可能是因为它主要被用在应用程序而不是网站中。大约有[38%的开发者宣称他们正在使用 React](https://www.sitepoint.com/front-end-tooling-trends-2017/#librariesandframeworks).

优点：
* 体积小，效率高，快速，灵活
* 简单的组件模型
* 良好的文档和线上资源
* 可以在服务器端渲染
* 当前非常受欢迎，正处于快速上升阶段

缺点：
* 需要学习新的概念和语法
* 必须依赖于构建工具才能使用
* 可能需要其它的库或框架来提供模型和控制器部分
* 可能与其它会改变 DOM 的代码和库不能兼容

### Lodash 和 Underscore
|Lodash||
|----|----|
|类型|库|
|网站|[lodash.com/](https://lodash.com/)|
|代码库|[github.com/lodash/lodash/](https://github.com/lodash/lodash/)|
|当前版本|4.17.4|
|开发者|John-David Dalton|
|发布日期|2012年4月|
|体积|压缩后 4kb - 24kb|
|用于|通用于各种项目|
|使用率|[低](https://w3techs.com/technologies/details/js-lodash/all/all)|

|Underscore||
|----|----|
|类型|库|
|网站|[underscorejs.org/](http://underscorejs.org/)|
|代码库|[github.com/jashkenas/underscore](https://github.com/jashkenas/underscore)|
|当前版本|1.8.3|
|开发者|Jeremy Ashkenas|
|发布日期|2009年10月|
|体积|压缩后 6kb|
|用于|通用于各种项目|
|使用率|[低](https://w3techs.com/technologies/details/js-underscore/all/all)|

之所以把 Lodash 和 Underscore 放在一起讨论，是因为它们同样都提供了数以百计的 JavaScript 实用函数作来弥补原生的字符串、数、数组以及其他 JavaScript 基本对象在方法上的的不足。它们在功能上有一些重叠，因此你不会在同一个项目中同时使用这两个库。

它们在客户端上的使用率并不是太高，但它们也都可以用在 Node.js 应用中，从而工作在服务器端。

优点：
* 小而简单
* 文档良好，易于学习
* 与大多数其它库与框架兼容
* 没有改变或扩展内建对象
* 在客户端和服务器端均可使用

缺点：
* 库中包含的一些功能已经在在 ES2015 及后续的 JavaScript 版本中引入，因此有一定程度的冗余

### AngularJS 1.x
|AngularJS||
|----|----|
|类型|框架|
|网站|[angularjs.org](https://angularjs.org/)|
|代码库|[github.com/angular/angular.js](https://github.com/angular/angular.js)|
|当前版本|1.6.4|
|开发者|Google|
|发布日期|2010年10月|
|体积|144kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-angularjs/all/all)|

Angular 是这份列表中出现的第一个框架（或者说MVC 应用框架）。其最流行的的版本是1.x，它扩展了 HTML 使之具备双向绑定能力，从而将 DOM 操作与应用程序逻辑解耦。

Angular 1.x仍然在持续开发中，但它也同时发布了 Angular 2（现在已经是 Angular 4!）。被搞糊涂了？请往下看……

优点：
* 得到了几个大公司的采用
* 它为构建现代 web 应用提供了一套单一而完整的解决方案
* 它是“标准”的 MEAN 技术栈（MongoDB、Express JS、AngularJS 和 NodeJS）中的一个组成部分

缺点：
* 与其它竞争者相比学习曲线更为陡峭
* 庞大的代码体积
* 无法升级到 Angular 2.x
* 尽管由 Google 开发，但 Google 并不在自己的产品中使用它？

### Angular 2.x（现在是4.x）
|Angular||
|----|----|
|类型|框架|
|网站|[angular.io](https://angular.io/)|
|代码库|[github.com/angular/angular.js](https://github.com/angular/angular.js)|
|当前版本|4.1|
|开发者|Google|
|发布日期|2016年9月|
|体积|压缩后 450kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-angularjs/all/all)|

Angular 2.0 发布于2016年9月。它名为2.0，其实是一个完全重写的版本。项目引入一个模块化的基于组件的模型，并用 TypeScript（再编译为 JavaScript）写成。2017年3月4.0版本发布（由于一些语义化版本上的问题，3.0版本被直接跳过了），这更增加了在版本上的混乱。

Angurlar 2 及其后续版本与其1.x版本是完全不同的两个项目，它们也互不兼容 —— 如果当时 Google 能给 Angular 2 另取一个不同的名字可能就不会这么令人费解了吧！

优点：
* 它也是一个用于构建现代 web 应用程序的单一而完整的解决方案
* 也仍是所谓 MEAN 技术栈的组成部分，尽管 Angular 2+ 的教程要比1.x版本少得多
* 对于那些熟悉 C# 和 Java 这种静态类型语言的人来说，用 TypeScript 作为开发语言也意味着是一种优点

缺点：
* 与其它竞争者相比学习曲线更为陡峭
* 庞大的代码体积
* 无法从 Angular 1.x 升级
* 与其 1.x 版本相比，人们对 Angular 2.x 的接受和使用程度要低得多
* 同样，尽管作为一个 Google 项目，但 Google 自己并没有使用它？

### Vue.js
|Vue.js||
|----|----|
|类型|框架|
|网站|[vuejs.org](https://vuejs.org/)|
|代码库|[github.com/vuejs/vue](https://github.com/vuejs/vue)|
|当前版本|2.0|
|开发者|Evan You|
|发布日期|2014年2月|
|体积|压缩后 19kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-vuejs/all/all)|

Vue.js 是一个用于构建用户界面的轻量级渐进式的框架。其核心部分提供类似 React 的虚拟 DOM 来驱动视图层，同时它可以与其它库进行整合，也完全可以独立搭建一个完整的单页应用程序。Evan You 创造了 Vue.js，他原本是 AngularJS 的使用者，但他从 AngularJS 中抽取了他喜欢的部分进而创建了 Vue.js。

Vue.js 使用 HTML 模板语法来绑定 DOM 和数据。其模型则是普通的 JavaScript 对象，当数据发生变化时，模型会去更新视图。它也提供了一些辅助性的工具，提供诸如脚手架、路由、状态管理及动画等功能。

优点：
* 人们接受它很快，并且受欢迎程度一直在增加
* 容易上手，开发者满意度高
* 依赖少，性能好

缺点：
* 还是一个比较新的项目 —— 这可能意味着较大的风险
* 依赖于作者一个人维护这个项目
* 与其它竞争者相比，资源较少

### Backbone.js
|Backbone.js||
|----|----|
|类型|框架|
|网站|[backbonejs.org](http://backbonejs.org/)|
|代码库|[github.com/jashkenas/backbone/](https://github.com/jashkenas/backbone/)|
|当前版本|1.3.3|
|开发者|Jeremy Ashkenas|
|发布日期|2010年10月|
|体积|压缩后 8kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-backbone/all/all)|

MVC 结构一般都出现在服务器端框架中，Backbone.js 则是最早提供客户端 MVC 的框架之一。它唯一的依赖就是其作者的另一个项目 Underscore.js。

Backbone.js 宣称自己是一个库，理由是它可以与其它项目整合。尽管我不像一些人那么武断，但我猜测大多数开发者都会认为它是一个框架。

优点：
* 体积小，轻量级，不那么复杂
* 不会向 HTML 中添加逻辑
* 文档非常好
* 被许多应用程序采用，例如 Trello、WordPress.com、LinkedIn 和 Groupon。

缺点：
* 与其它竞争者像 AngularJS 相比，抽象的层次较低（尽管这可能也是个优点）
* 需要由额外的组件来实现像数据绑定这样的功能
* 现在越来越多的框架已经不再使用 MVC 架构

### Ember.js
|Ember.js||
|----|----|
|类型|框架|
|网站|[emberjs.com](https://emberjs.com/)|
|代码库|[github.com/emberjs/ember.js](https://github.com/emberjs/ember.js)|
|当前版本|2.15.0|
|开发者|Ember team|
|发布日期|2011年12月|
|体积|压缩后 95kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-emberjs/all/all)|

Ember.js 是基于模型—视图—视图模型（MVVM）模式的大型框架之一。它在一个单一包内实现了模板、数据绑定以及其它库的功能。具有 Ruby on Rails 经验的人会对它所倡导的约定优于配置的概念感到很熟悉。

优点：
* 为客户端应用开发提供了单一解决方案
* 由于它使用了 jQuery，开发者会很快上手
* 良好的向后兼容性和升级选项
* 采用了现代 web 开发的标准

缺点：
* 分发包的体积大
* 相对于其它框架正在朝着小型组件化结构的方向变化，人们认为 Ember.js 比较庞大且不可分割
* 学习曲线更为陡峭

### Knockout.js
|Knockout.js||
|----|----|
|类型|框架|
|网站|[knockoutjs.com](http://knockoutjs.com/)|
|代码库|[github.com/knockout/knockout](https://github.com/knockout/knockout)|
|当前版本|3.4.2|
|开发者|Steve Sanderson|
|发布日期|2010年7月|
|体积|压缩后 59kb|
|用于|单页应用|
|使用率|[低](https://w3techs.com/technologies/details/js-knockout/all/all)|

Knockout.js 是较老的 MVVM 框架之一。它使用观察者模式来保证用户界面与数据保持同步。它的特色在于其模板和依赖跟踪。

优点：
* 体积小，轻量级，无依赖
* 优异的浏览器兼容性，甚至支持 IE6
* 良好的文档

缺点：
* 在较大型项目中使用它可能会使项目变得比较复杂
* 框架本身的开发已经放慢了
* 使用率已经出现衰落

### 其它值得关注的项目
还想了解更多的项目吗？下面这些项目的流行程度可能不如上面提到的那些，但也是值得关注的：
* [Polymer](https://www.polymer-project.org/) ：一个使浏览器支持 HTML5 web 组件的库，并且可以跨浏览器工作
* [Meteor](https://www.meteor.com/) ：一个用于开发 web 应用程序的全栈式平台
* [Aurelia](http://aurelia.io/) ：一个非常新的、轻量级的、跨平台的框架
* [Svelte](https://svelte.technology/) ：一个非常新的项目，可以将框架代码转换成清晰的 JavaScript 代码
* [Conditioner.js](http://conditionerjs.com/) ：一个新的库，能够根据状态自动加载和卸载模块

## 工具：任务执行
构建工具可以自动运行 web 开发过程中各种不同的任务，例如预处理、编译、图片优化、代码压缩、代码检查以及运行测试。这些任务可以统一由一个单独的可执行包来管理。最受人欢迎的选项包括：
### Gulp.js
|Gulp.js||
|----|----|
|网站|[gulpjs.com](http://gulpjs.com/)|
|代码库|[github.com/gulpjs/gulp](https://github.com/gulpjs/gulp)|
|当前版本|3.9.1|
|月下载量|300万|

Gulp 尽管并非第一个任务执行工具，但它迅速成为了最受欢迎的选择并且[我个人也非常喜欢它](https://www.sitepoint.com/introduction-gulp-js/?aref=cbuckler)。Gulp 通过非常易读的 JavaScript 代码将源文件加载到流中，并将其通过管道在不同插件之间流转，然后输出。它很简单、快速而且有趣 —— 建议你在选择其它选项之前一定要试一试 Gulp.js。
### npm
|npm||
|----|----|
|网站|[npmjs.com](https://www.npmjs.com/)|
|代码库|[github.com/npm/npm](https://github.com/npm/npm)|
|当前版本|4.5.0|
|月下载量|300万|

npm 是 Node.js 的包管理器，但它在脚本方面的能力可以被用于[任务执行](https://www.sitepoint.com/guide-to-npm-as-a-build-tool/?aref=cbuckler)。npm 脚本对于依赖较少的简单项目非常有吸引力，但当面对复杂任务时它很快就会变成了一个不可行的选择了。
### Grunt
|Grunt||
|----|----|
|网站|[gruntjs.com](https://gruntjs.com/)|
|代码库|[github.com/gruntjs/grunt](https://github.com/gruntjs/grunt)|
|当前版本|1.0.1|
|月下载量|200万|

Grunt 是最早被大范围使用的 JavaScript 任务执行器之一，但是由于运行速度不佳和 JSON 配置复杂，导致被 Gulp 赶超。现在的 Grunt 已经解决了这些原本很糟糕的问题，所以它仍然是一个受欢迎的选择。
## 工具：模块打包
近年来管理大量 JavaScript 文件已经迅速成了每个项目中的例行任务。默认情况下，在浏览器中文件不会被编译，因此必须按照一定的顺序加载或者连接合并所有的依赖文件。对此现在有像 [ES6 模块](https://www.sitepoint.com/understanding-es6-modules/?aref=cbuckler)和 [CommonJS](http://wiki.commonjs.org/wiki/Modules) 这样的解决方案，但浏览器对它们的支持并不好，所以一个模块打包工具就变得必不可少。
### Webpack
|Webpack||
|----|----|
|网站|[webpack.js.org](https://webpack.js.org/)|
|代码库|[github.com/webpack/webpack](https://github.com/webpack/webpack)|
|当前版本|2.5.1|
|月下载量|600万|

Webpack 支持所有流行的模块形式，已经变成了 React 开发过程的标配。Webpack 虽然宣称自己是一个模块打包工具，但它也可以被用作一般的任务执行工具。
### Browserify
|Browserify||
|----|----|
|网站|[browserify.org](http://browserify.org/)|
|代码库|[github.com/substack/node-browserify](https://github.com/substack/node-browserify)|
|当前版本|14.3.0|
|月下载量|260万|

Browserify 支持 Node.js 所使用的 CommonJS 模块，它把所有模块编译成一个单独的可在浏览器中执行的文件。
### RequireJS
|RequireJS||
|----|----|
|网站|[requirejs.org](http://requirejs.org/)|
|代码库|[github.com/jrburke/r.js](https://github.com/jrburke/r.js)|
|当前版本|2.3.3|
|月下载量|100万|

RequireJS 是一个运行在浏览器中的模块加载工具，它也可以在 Node.js 中使用。
## 工具：代码检查
代码检查工具可以通过分析你的代码来发现潜在的错误和违反编码规范之处。有了它的帮助，你再也不会忘记关闭括号或者忘记在使用前声明变量了！
### ESLint
|ESLint||
|----|----|
|网站|[eslint.org](http://eslint.org/)|
|代码库|[github.com/eslint/eslint](https://github.com/eslint/eslint)|
|当前版本|3.19.0|
|月下载量|600万|

ESLint 是一个可插拔式的代码检查工具，每条规则都是一个插件，因此你可以把它配置成你喜欢的样子。
### JSHint
|JSHint||
|----|----|
|网站|[jshint.com](http://jshint.com/)|
|代码库|[github.com/jshint/jshint](https://github.com/jshint/jshint)|
|当前版本|2.9.4|
|月下载量|200万|

JSHint 是一个灵活的JavaScript 代码检查工具，它在真正的代码错误和迂腐的句法要求之间做到了一个很好的平衡。它也是我个人的最爱。
### JSLint
|JSLint||
|----|----|
|网站|[jslint.com](http://jslint.com/)|
|代码库|[github.com/reid/node-jslint](https://github.com/reid/node-jslint)|
|当前版本|0.10.3|
|月下载量|5万|

它是最早的代码检查工具之一，实现了一套严格的默认规则。但对我来说它的规则有点太过严苛了。
## 工具：测试套件
测试驱动开发要求你在开始写你的代码之前先写测试代码。我们还鼓励你写测试你的测试代码的代码！

这方面的选择有很多包括 [Ava](https://ava.li/)、[Tape](https://github.com/substack/tape) 和 [Jest](http://facebook.github.io/jest/)。但最受欢迎选项有：
### Mocha
|Mocha||
|----|----|
|网站|[mochajs.org](https://mochajs.org/)|
|代码库|[github.com/mochajs/mocha](https://github.com/mochajs/mocha)|
|当前版本|3.3.0|
|月下载量|500万|

Mocha 是一个 JavaScript 测试框架，可以在 Node.js 或浏览器环境中运行测试。它支持异步测试，还经常与 [Chai](http://chaijs.com/) 一起使用来写出更为易读的测试代码。
### Jasmine
|Jasmine||
|----|----|
|网站|[jasmine.github.io](https://jasmine.github.io/)|
|代码库|[github.com/jasmine/jasmine-npm](https://github.com/jasmine/jasmine-npm)|
|当前版本|2.6.0|
|月下载量|200万|

Jasmine 是一个行为驱动测试套件，它可以在浏览器中自动化测试你的用户界面与交互。
### QUnit
|QUnit||
|----|----|
|网站|[https://qunitjs.com/](https://qunitjs.com/)|
|代码库|[github.com/kof/node-qunit](https://github.com/kof/node-qunit)|
|当前版本|1.0.0|
|月下载量|2.5万|

顾名思义，QUnit 是一个单元测试框架，它可以测试当传入的特定参数时你的函数的返回结果。它还可以报告测试覆盖率以确保你不会遗漏一些代码流程分支。
## 工具：杂项
尽管我在这里尽了自己最大的努力，但我也承认并非每个人都喜欢 JavaScript！像 [TypeScript](https://www.typescriptlang.org/)、[LiveScript](http://livescript.net/)和 [CoffeeScript](http://coffeescript.org/) 这样的编译到 JavaScript 的语言可以让你的开发生活更为愉悦。而作为替代方案，也可以考虑使用 [Babel](https://babeljs.io/) 来把现代的、简洁的 [ES2015](https://www.sitepoint.com/premium/courses/diving-into-es2015-2924) 代码转换成跨浏览器兼容的 ES5 代码。

[基于 JavaScript 的 HTML 模板引擎](https://www.sitepoint.com/overview-javascript-templating-engines/?aref=cbuckler)也有很多，例如 [Mustache](https://mustache.github.io/)、[Handlebars](http://handlebarsjs.com/)、[Pug (Jade)](https://pugjs.org/api/getting-started.html) 和 [EJS](http://embeddedjs.com/)。我比较喜欢保留了 JavaScript 语法的轻量级的模板引擎，比如 [EJS](http://ejs.co/) 和 [doT](https://olado.github.io/doT/)。

最后，如果可以自动生成文档的话你为什么还要自己去手写呢？与 ES2015 兼容的文档生成工具主要有 [ESDoc](https://esdoc.org/)、[JSDoc](http://usejsdoc.org/)、[YUIdoc](http://yui.github.io/yuidoc/)、[documentation.js](http://documentation.js.org/) 和 [Transcription](https://github.com/affirmix/transcription)。
## 总结与推荐
如果你选择相信众人的智慧，那么现在的势头正盛的是 [React](https://facebook.github.io/react/)，而其他库也都在朝着类似的技术方向发展。对于 web 应用程序而言，React 是一个安全、通用的选择，但你也不妨考虑一下 [Vue.js](https://vuejs.org/)。

那种大一统的框架已经逐渐在人们心目中失宠，但如果你需要为一个大型项目选择一个严谨的架构，[AngularJS](https://angularjs.org/) 仍然是一个受欢迎的选择。大多数使用者都还在坚持使用1.0版本但这恐怕是他们不得不这么做而不是他们想要这样。长远来看，选择4+版本会更为安全，但前提是你得愿意学习 TypeScript。

不要忽视 [jQuery](http://jquery.com/)。尽管它已经不再时髦且很少再被技术出版物提及，但它仍然发展得很活跃且对于网站和应用程序来说都很适用。jQuery 的学习曲线平缓，世界上很多开发者都会使用它。

如果你认为自己富有冒险精神，[Svelte](https://svelte.technology/) 会是一个很有趣的客户端和服务器端选择。它在编译阶段提前渲染 JavaScript，对我们以往传统的开发方式是一个很大的改变。

与库和框架相比，工具的选择就显得不是那么关键了，不同的项目可能会有很多不同的选择。大多数人在使用 [Gulp](http://gulpjs.com/)，但越来越多的人开始喜欢 [Webpack](https://webpack.js.org/)。有 [ESLint](http://eslint.org/) 做代码检查，再加上 [Mocha](https://mochajs.org/) 进行测试，你不再会写出错误的代码。当然，它们也有都很多可替代的选择。

人们总是说每个项目、每个团队包括他们的技术积累都是不同的。你并没有太多的时间去做技术选择与评估，因此你总是习惯于去使用你所了解的东西。在这篇文章下面肯定会有很多评论推荐别的 XXX 框架更好，因为当你有一把锤子的时候，总是会看着每样东西都像钉子。

最后，请记住这些库、框架和工具都是可选的，而并非必须的。在过去10年里 JavaScript 开发已经发生了翻天覆地的变化，我们已经从当初只有几个不成熟的库发展到现在这个选择过剩的时代。我们会很容易落入复杂性不断增长的陷阱，或者没几个月就换用一个更新更火的框架。当你在做一些小的或者个人项目时，请考虑使用原生的 JavaScript。只有这方面的知识积累才是永远不会过时的，并且会在你为别的项目选择框架时产生非常宝贵的价值。

我遗漏了你喜欢的 JavaScript 库、框架或者工具了吗？我想这是肯定的！那么就请在下面给出你的评论吧。
