# Flutter | 深入理解BuildContext

# 前言
最近看到一些刚接触Flutter的同学在进行页面跳转的时候，出现了这个问题。
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
一眼看上去好像没什么问题，解决方式也很简单，把home部分作为一个新的Widget拆出来就可以了。
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
但是刚开始遇到这些东西的时候一定是很懵逼的。BuildContext是什么鬼，为什么每次我们需要在build函数的时候传入一个BuildContext？为什么我的Navigator操作会出现当前的context找不到Navigator的情况，为什么拆成新的widget就好了？

所以今天想顺着这个问题跟大家分享一下如何在Flutter中理解和使用BuildContext。其中还会涉及到一些widget构建流程的地方，在正式开始之前先简单解释一下这几个概念。

### 什么是Navigator，MaterialApp做了什么
我们经常会在应用中打开许多页面，当我们返回的时候，它会先后退到上一个打开的页面，然后一层一层后退，没错这就是一个堆栈。而在Flutter中，则是由Navigator来负责管理维护这些页面堆栈。
``` dart
压一个新的页面到屏幕上
Navigator.of(context).push
把路由顶层的页面移除
Navigator.of(context).pop
```
通常我们我们在构建应用的时候并没有手动去创建一个Navigator，也能进行页面导航，这又是为什么呢。

没错，这个Navigator正是MaterialApp为我们提供的。但是如果home，routes，onGenerateRoute和onUnknownRoute都为null，并且builder不为null，MaterialApp则不会创建任何Navigator。

知道了Navigator和MaterialApp发挥的作用之后，我们再来看看BuildContext。

## BuildContext
每次我们在编写界面部分代码的时候，都是在build函数中进行操作。而build函数则需要默认传入一个BuildContext。我们来看看这到底是啥。
``` dart
abstract class BuildContext {
  /// The current configuration of the [Element] that is this [BuildContext].
  Widget get widget;

  /// The [BuildOwner] for this context. The [BuildOwner] is in charge of
  /// managing the rendering pipeline for this context.
  BuildOwner get owner;
  ...
```
我们可以看到BuildContext其实是一个抽象类，但是每次build函数传进来的是什么呢。我们来看看构建视图的时候到底发生了什么。

### Flutter如何构建视图
在Flutter中，Everything is Widget，我们通过构造函数嵌套Widget来编写UI界面。实际上，Widget并不是真正要显示在屏幕上的东西,只是一个配置信息，它永远是immutable的，并且可以在多处重复使用。那真正显示在屏幕上的视图树是什么呢？Element Tree！

那我们来看一下，在构建视图的时候究竟发生了什么。这里以Stateless Widget为例。
``` dart
abstract class StatelessWidget extends Widget {
  const StatelessWidget({ Key key }) : super(key: key);
  @override
  StatelessElement createElement() => StatelessElement(this);
  ...
```
当要把这个widget装进视图树的时候，首先会去createElement，并将当前widget传给Element。

我们再来看一看这个StatelessElement是什么
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
我们可以看到，通过将widget传入StatelessElement的构造函数，StatelessElement保留了widget的引用，并且将会调用build方法。

而这个build方法真正调用的则是widget的build方法，并将this，也就是该StatelessElement对象传入。我们知道，build方法需要传入的是一个BuildContext，为什么传进去了StatelessElement？于是我们继续看。
``` dart
class StatelessElement extends ComponentElement
...
abstract class ComponentElement extends Element
...
abstract class Element extends DiagnosticableTree implements BuildContext 
```
实际上是Element类实现了BuildContext，并由ComponentElement  -> StatelessElement 继承。

所以我们现在再来看官方对于BuildContext的解释:

**BuildContext**objects are actually **Element** objects. The **BuildContext**interface is used to discourage direct manipulation of **Element** objects.


BuildContext对象实际上就是Element对象，BuildContext 接口用于阻止对 Element 对象的直接操作。

Cool！我们现在终于知道这个BuildContext是哪里来的了。让我们再来梳理一下，flutter构建视图究竟做了什么。

### 视图树装载过程
#### StatelessWidget
- 首先它会调用StatelessWidget的 createElement 方法，并根据这个widget生成StatelesseElement对象。
- 将这个StatelesseElement对象挂载到element树上。
- StatelesseElement对象调用widget的build方法，并将element自身作为BuildContext传入。

#### StatefulWidget
- 首先同样也是调用StatefulWidget的 createElement方法，并根据这个widget生成StatefulElement对象，并保留widget引用。
- 将这个StatefulElement挂载到Element树上。
- 根据widget的 createState 方法创建State。
- StatefulElement对象调用state的build方法，并将element自身作为BuildContext传入。

所以我们在build函数中所使用的context，正是当前widget所创建的Element对象。

## of(context)方法
在flutter中我们经常会使用到这样的代码
``` dart
//打开一个新的页面
Navigator.of(context).push
//打开Scaffold的Drawer
Scaffold.of(context).openDrawer
//获取display1样式文字主题
Theme.of(context).textTheme.display1
```
那么这个of(context)到底是个什么呢。我们这里以Navigator打开新页面为例。
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
可以看到，关键代码部分通过context.rootAncestorStateOfType向上遍历 Element tree，并找到最近匹配的 NavigatorState。也就是说of实际上是对context跨组件获取数据的一个封装。

而我们的Navigator的 push操作就是通过找到的 NavigatorState 来完成的。

不仅如此，BuildContext还有许多方法可以跨组件获取对象
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
需要注意的是，在 State 中 initState阶段是无法跨组件拿数据的，只有在didChangeDependencies之后才可以使用这些方法。

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
当我们在 build 函数中使用Navigator.of(context)的时候，这个context实际上是通过 MyApp 这个widget创建出来的Element对象，而of方法向上寻找祖先节点的时候（MyApp的祖先节点）并不存在MaterialApp，也就没有它所提供的Navigator。

所以当我们把Scaffold部分拆成另外一个widget的时候，我们在FirstPage的build函数中，获得了FirstPage的BuildContext，然后向上寻找发现了MaterialApp，并找到它提供的Navigator，于是就可以愉快进行页面跳转了。

## 参考资料
- [Flutter Widgets101](https://www.youtube.com/watch?v=wE7khGHVkYY&index=2&list=PLOU2XLYxmsIJyiwUPCou_OVTpRIn_8UMd)：Flutter团队官方视频，介绍了statelessWidget与StatefulWidget究竟是怎么被创建的，推荐观看。

# 写在最后
文章若有不对之处还请各位高手指出，欢迎在下方评论区以及我的邮箱1652219550a@gmail.com留言，我会在24小时内与您联系！
