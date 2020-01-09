# Flutter 的构建模式

[![JAY TILLU](https://miro.medium.com/fit/c/96/96/2*li4M13D8csNZa-aV6jWloQ.jpeg)](/@jaytillu?source=post_page-----b44f179ad718----------------------)

[JAY TILLU](/@jaytillu?source=post_page-----b44f179ad718----------------------)

![](https://miro.medium.com/max/60/1*-c-1Cr0fgC6RV8AfUH6fRA.png?q=20)

<img class="ds t u gf ak" src="https://miro.medium.com/max/4000/1\*-c-1Cr0fgC6RV8AfUH6fRA.png" width="2000" height="1000" role="presentation"/>

要完全掌握一款框架，我们需要明白它内部的工作机制。我们必须弄清楚**它是如何运行的**、**为什么会这样**。就像人与人之间，_彼此了解越多，彼此越懂对方_。那么让我们试着了解一些 Flutter 内部细节以及它的构建模式。

通过这篇文章，我们将了解到：

*   什么是构建模式？
*   Flutter 中有几种构建模式？
*   我们为什么需要构建模式？

# 什么是构建模式？

> 在不同的开发阶段，框架用不同的方式，或者叫模式，来编译我们的代码，这里的模式我们就称为构建模式。

正如大家都知道的在某个时间点后，维护并调整代码真的很难很费时间。你去问任何一个移动应用开发者，他们都会告诉你这里的痛点。如果你在维护一个像 Facebook、Instagram、Uber 或是 WhatsApp 这样的巨型应用，那么哪怕仅仅是修改一个颜色，都需要花费**数小时**才能看到变化。如果你做 Android 开发，情况会很糟糕。

但 Flutter 的到来会让情况变好。让我们看看它是怎么做到的。

Flutter 团队重新设计了整个开发过程，致力于提升编译速度，使其快如闪电。当你按下编译按钮，其他框架会重新编译全部代码，即使你只是做了很小的修改，也会耗费很长时间，但Flutter 不会那样。

比起一次又一次地重新编译整个代码，Flutter 团队把编译阶段分成三种模式。第一种模式适用开发 app 期间，第二种模式适用测试 app 性能，第三种模式适用 app 发布。通过将编译过程分成不同的模式，Flutter 能够以快如闪电的速度反应代码的变化。接下来让我们来了解下这些模式。

# 一共有多少种模式？应该如何选用呢？

*   大体上说，Flutter 有三种构建模式：

1.  Debug 模式
2.  Profile 模式
3.  Release 模式

# Debug 模式

> Debug 模式被设计用于快速反应变化，但 app 的体积很大，性能很差。因此不该评判在此模式下 app 的体积和性能。

*   作为开发流程中的首个阶段，app 开发阶段中开发者会修改很多代码，修 bug，随处修改。因此开发者需要随时看到变化。
*   因此 debug 模式中，Flutter 优化了所见所得的速度。
*   Dart 语言帮助了 Flutter 更快地反馈变化。Dart 代码可以通过多种方式运行。在 debug 模式下，Flutter 在一个虚拟机中执行代码。在你作出修改然后触发 hot reload 时，Flutter 会把变化的代码注入 app 中，不需要重新编译。
*   就像 web 开发一样，当你改了一些代码，然后按下刷新按钮，在浏览器里马上能看到变化，这其中的概念是一样的。你运行着的 app 代码发生变化，Flutter 会把变化反应到 app 上。
*   Flutter 不重新编译所有代码，就可以立即反馈给你变化，因此加快了开发速度，提升效率。
*   由于 app 运行在虚拟机中，性能是最差的。所以你会感觉在 debug 模式下运行的 app 很慢。但此时 Flutter 专门为及时响应变化而做出优化，我们不该评判此时的 app 性能。
*   同时 debug 模式下 app 看起来很大，所以也不要评判此时的 app 体积，因为它正运行在 debug 模式下……

**当你在 debug 模式下，同时请多关注这些重要的事。**

*   [断言机制](https://dart.dev/guides/language/language-tour#assert) 已被启用。
*   Service extensions 已被启用。
*   调试工具已被启用。(你可以使用 [DevTools](https://flutter.dev/docs/development/tools/devtools/overview))。
*   你可以在真机、模拟器上运行 app。

# Profile 模式

> Profile 模式用于在测试时分析性能。

*   当 app 在 Profile 模式下编译时，Flutter 会假定你要评测你的 app 性能。因此 app 会尽可能地针对运行性能做优化。
*   此时你的 app 已经开发完成，你可以分析 app 的实际性能。

**当你在 profile 模式下，同时请多关注这些重要的事。**

*   在 profile 模式下，你不能在模拟器中运行 app。因为并没有针对模拟器做出优化，在模拟器中 profile 模式是不可用的。
*   Tracing 功能已被启用。
*   一些用于分析性能的 service extensions 已被启用。
*   对 DevTools 的支持也被启用了。

**_Flutter 推荐使用_** [**_DevTools_**](https://flutter.dev/docs/development/tools/devtools) **_来分析应用性能。_**

# Release 模式

> Released 模式被设计用于发布到 Play Store 或 App Store。这种模式旨在提供更快的启动速度、执行速度和最小应用体积。

*   此时应用已经开发、测试结束了。是时候在 release 模式下编译了。Flutter 会通过 AOT 编译器把代码编译成 native 机器码，因此 app 会运行地很快。要了解编译器的更多内容，请[点击这里](/flutter-widgets/3-flutter-compilation-process-e602d939e147)。
*   当 app 在 Release 模式下编译时，Flutter 会假定你要准备发布 app 了，所以优化到了极限。
*   在 release 模式下，专门优化了启动速度、执行速度和尽可能小的 app 体积。

**当你在 release 模式下，同时请多关注这些重要的事。**

*   断言机制被禁用了。
*   调试信息被剥离掉了。
*   调试功能不可用。
*   Service extension 也被禁用了
*   Release 模式不支持模拟器。

# 想了解更多信息请访问以下链接

*   [Fuchsia OS 官方网站](https://fuchsia.dev/)
*   [Dart 官方网站](https://dart.dev/)
*   [Flutter 官方网站](https://flutter.dev/)

> 这就是关于构建模式的一些内容，希望能够帮助到你。如果我遗漏了什么请告诉我。保持 coding，保持心里有爱。
> 
> 想和我交流？以下是我的联系方式。我很愿意与你成为朋友。😊
> 
> [Twitter](https://twitter.com/jay_tillu)
> 
> [Facebook](https://www.facebook.com/jaytillu.1314/)
> 
> [Instagram](https://www.instagram.com/jay.tillu/)
> 
> 或者给我发邮件：developerj13@gmail.com
