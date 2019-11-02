# 如何判断一个界面是 Flutter 构建的

总有开发者说：这种效果肯定不是 Flutter 做的。问其原因，他说 Flutter 做的肯定能看出来嘛（意思就是，Flutter 做的效果毕竟不能和 Native 的效果媲美）。结局当然是这位开发者被打脸，Native App 的效果，Flutter 基本都能做出来。

那么怎么样去判断一个界面是不是 Flutter 构建的呢？**最简单的一种办法**，用两指滑动屏幕上的滚动列表，如果此时滚动列表以 2 倍的速度滚动，那么这极大概率是用 Flutter 构建的。用三个四个五个手指滚动呢，那滚动速度就是 3 倍、4 倍、5 倍。这个小技巧对于判断应用的 Flutter 使用状况是非常有效的，你可以轻易地了解一款 App 的什么样的场景和业务使用了 Flutter 构建。

前文讲到的技巧，基于一个假设，假设这里有一个滚动列表可供你进行尝试。如果是一个不含可滚动列表的 Flutter 界面，我们如何判断呢。Flutter 的实现机制告诉我们，Flutter 只会有一层 Native 的“画布”，因此可以通过 FLEX、Lookin、Reveal 等工具确定页面上所有元素均绘制在一层 FlutterView 上。

![](tip_how_to_find_flutter_page_01.png)