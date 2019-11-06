# Flutter | 深入理解BuildContext

# 前言
最近看到一些刚接触 Flutter 的同学在进行页面跳转的时候，出现了这个问题。
```
flutter: Navigator operation requested with a context that does not include a Navigator.
flutter: The context used to push or pop routes from the Navigator must be that of a widget that is a
flutter: descendant of a Navigator widget.
```
代码是这样的
``` dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: FlatButton(
              onPressed: () {
                Navigator.of(context).push(
                    MaterialPageRoute(builder: (context) => SecondPage()));
              },
              child: Text('跳转')),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
    );
  }
}
```
一眼看上去好像没什么问题，解决方式也很简单，把 home 部分作为一个新的 Widget 拆出来就可以了。
``` dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: FirstPage(),
    );
  }
}

class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
          child: FlatButton(
              onPressed: () {
                Navigator.of(context).push(
                    MaterialPageRoute(builder: (context) => SecondPage()));
              },
              child: Text('跳转')),
        ),
      );
  }
}
```
但是刚开始遇到这些东西的时候一定是很懵逼的。BuildContext 是什么鬼，为什么每次我们需要在 build 函数的时候传入一个 BuildContext？为什么我的 Navigator 操作会出现当前的 context 找不到 Navigator 的情况，为什么拆成新的 widget 就好了？

所以今天想顺着这个问题跟大家分享一下如何在 Flutter 中理解和使用 BuildContext。其中还会涉及到一些 widget 构建流程的地方，在正式开始之前先简单解释一下这几个概念。

### 什么是 Navigator，MaterialApp 做了什么
我们经常会在应用中打开许多页面，当我们返回的时候，它会先后退到上一个打开的页面，然后一层一层后退，没错这就是一个堆栈。而在 Flutter 中，则是由 Navigator 来负责管理维护这些页面堆栈。
``` dart
//压一个新的页面到屏幕上
Navigator.of(context).push
//把路由顶层的页面移除
Navigator.of(context).pop
```
通常我们我们在构建应用的时候并没有手动去创建一个 Navigator，也能进行页面导航，这又是为什么呢。

没错，这个 Navigator 正是 MaterialApp 为我们提供的。但是如果 home，routes，onGenerateRoute 和 onUnknownRoute 都为 null，并且 builder 不为 null，MaterialApp 则不会创建任何 Navigator。

知道了 Navigator 和 MaterialApp 发挥的作用之后，我们再来看看 BuildContext。

## BuildContext
每次我们在编写界面部分代码的时候，都是在 build 函数中进行操作。而 build 函数则需要默认传入一个 BuildContext。我们来看看这到底是啥。
``` dart
abstract class BuildContext {
  /// The current configuration of the [Element] that is this [BuildContext].
  Widget get widget;

  /// The [BuildOwner] for this context. The [BuildOwner] is in charge of
  /// managing the rendering pipeline for this context.
  BuildOwner get owner;
  //...
```
我们可以看到 BuildContext 其实是一个抽象类，但是每次 build 函数传进来的是什么呢。我们来看看构建视图的时候到底发生了什么。

### Flutter 如何构建视图
在 Flutter 中，Everything is Widget，我们通过构造函数嵌套 Widget 来编写 UI 界面。实际上，Widget 并不是真正要显示在屏幕上的东西，只是一个配置信息，它永远是 immutable 的，并且可以在多处重复使用。那真正持有“屏幕上的视图”/“UI控件”树是是什么呢？Element tree！

那我们来看一下，在构建视图的时候究竟发生了什么。这里以 Stateless Widget 为例。
``` dart
abstract class StatelessWidget extends Widget {
  const StatelessWidget({ Key key }) : super(key: key);
  @override
  StatelessElement createElement() => StatelessElement(this);
  //...
```
当要把这个 widget 装进视图树的时候，首先会去 createElement，并将当前 widget 传给 Element。

我们再来看一看这个 StatelessElement 是什么
``` dart
class StatelessElement extends ComponentElement {
  /// Creates an element that uses the given widget as its configuration.
  StatelessElement(StatelessWidget widget) : super(widget);

  @override
  StatelessWidget get widget => super.widget;

  @override
  Widget build() => widget.build(this);

  @override
  void update(StatelessWidget newWidget) {
    super.update(newWidget);
    assert(widget == newWidget);
    _dirty = true;
    rebuild();
  }
}
```
我们可以看到，通过将 widget 传入 StatelessElement 的构造函数，StatelessElement 保留了 widget 的引用，并且将会调用 build 方法。

而这个 build 方法真正调用的则是 widget 的 build 方法，并将 this，也就是该 StatelessElement 对象传入。我们知道，build 方法需要传入的是一个 BuildContext，为什么传进去了 StatelessElement？于是我们继续看。
``` dart
class StatelessElement extends ComponentElement
//...
abstract class ComponentElement extends Element
//...
abstract class Element extends DiagnosticableTree implements BuildContext 
```
实际上是 Element 类实现了 BuildContext，并由 ComponentElement  -> StatelessElement 继承。

所以我们现在再来看官方对于 BuildContext 的解释:

**BuildContext**objects are actually **Element** objects. The **BuildContext**interface is used to discourage direct manipulation of **Element** objects.


BuildContext 对象实际上就是 Element 对象，BuildContext 接口用于阻止对 Element 对象的直接操作。

Cool！我们现在终于知道这个 BuildContext 是哪里来的了。让我们再来梳理一下，flutter 构建视图究竟做了什么。

### 视图树装载过程
#### StatelessWidget
- 首先它会调用 StatelessWidget的 createElement 方法，并根据这个 widget 生成 StatelesseElement 对象。
- 将这个 StatelesseElement 对象挂载到 element 树上。
- StatelesseElement 对象调用 widget 的 build 方法，并将 element 自身作为 BuildContext 传入。

#### StatefulWidget
- 首先同样也是调用 StatefulWidget 的 createElement 方法，并根据这个 widget 生成 StatefulElement 对象，并保留 widget 引用。
- 将这个 StatefulElement 挂载到 Element 树上。
- 根据 widget的 createState 方法创建 State。
- StatefulElement 对象调用 state 的 build 方法，并将 element 自身作为 BuildContext 传入。

所以我们在 build 函数中所使用的 context，正是当前 widget 所创建的 Element 对象。

## of(context)方法
在 flutter 中我们经常会使用到这样的代码
``` dart
//打开一个新的页面
Navigator.of(context).push
//打开Scaffold的Drawer
Scaffold.of(context).openDrawer
//获取display1样式文字主题
Theme.of(context).textTheme.display1
```
那么这个 of(context) 到底是个什么呢。我们这里以 Navigator 打开新页面为例。
``` dart
static NavigatorState of(
    BuildContext context, {
      bool rootNavigator = false,
      bool nullOk = false,
    }) {
//关键代码-----------------------------------------v
    
    final NavigatorState navigator = rootNavigator
        ? context.rootAncestorStateOfType(const TypeMatcher<NavigatorState>())
        : context.ancestorStateOfType(const TypeMatcher<NavigatorState>());
        
//关键代码----------------------------------------^
    assert(() {
      if (navigator == null && !nullOk) {
        throw FlutterError(
          'Navigator operation requested with a context that does not include a Navigator.\n'
          'The context used to push or pop routes from the Navigator must be that of a '
          'widget that is a descendant of a Navigator widget.'
        );
      }
      return true;
    }());
    return navigator;
  }
```
可以看到，关键代码部分通过 context.rootAncestorStateOfType 向上遍历 Element tree，并找到最近匹配的 NavigatorState。也就是说 of 实际上是对 context 跨组件获取数据的一个封装。

而我们的 Navigator 的 push 操作就是通过找到的 NavigatorState 来完成的。

不仅如此，BuildContext 还有许多方法可以跨组件获取对象
```
ancestorInheritedElementForWidgetOfExactType(Type targetType) → InheritedElement

ancestorRenderObjectOfType(TypeMatcher matcher) → RenderObject

ancestorStateOfType(TypeMatcher matcher) → State

ancestorWidgetOfExactType(Type targetType) → Widget

findRenderObject() → RenderObject

inheritFromElement(InheritedElement ancestor, { Object aspect }) → InheritedWidget

inheritFromWidgetOfExactType(Type targetType, { Object aspect }) → InheritedWidget

rootAncestorStateOfType(TypeMatcher matcher) → State

visitAncestorElements(bool visitor(Element element)) → void

visitChildElements(ElementVisitor visitor) → void
```
需要注意的是，在 State 中 initState 阶段是无法跨组件拿数据的，只有在 didChangeDependencies 之后才可以使用这些方法。

## 回顾问题
我们现在再来看看之前遇到的 当前 context 不包含 Navigator 这个问题是不是很简单了呢。
``` dart
class  MyApp  extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: FlatButton(
              onPressed: () {
                Navigator.of(context).push(
                    MaterialPageRoute(builder: (context) => SecondPage()));
              },
              child: Text('跳转')),
        ),
      ),
    );
  }
}
```
当我们在 build 函数中使 Navigator.of(context) 的时候，这个 context 实际上是通过 MyApp 这个 widget 创建出来的 Element 对象，而 of 方法向上寻找祖先节点的时候（MyApp的祖先节点）并不存在 MaterialApp，也就没有它所提供的 Navigator。

所以当我们把 Scaffold 部分拆成另外一个 widget 的时候，我们在 FirstPage 的 build 函数中，获得了 FirstPage 的 BuildContext，然后向上寻找发现了 MaterialApp，并找到它提供的 Navigator，于是就可以愉快进行页面跳转了。

## 参考资料
- [Flutter Widgets101](https://www.youtube.com/watch?v=wE7khGHVkYY&index=2&list=PLOU2XLYxmsIJyiwUPCou_OVTpRIn_8UMd)：Flutter 团队官方视频，介绍了 statelessWidget 与 StatefulWidget 究竟是怎么被创建的，推荐观看。

# 写在最后
文章若有不对之处还请各位高手指出，欢迎在下方评论区以及我的邮箱1652219550a@gmail.com留言，我会在24小时内与您联系！
