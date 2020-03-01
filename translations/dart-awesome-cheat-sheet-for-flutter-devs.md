---
title: Flutter 开发者的 Dart 速查表
---

原文作者：[![Temidayo Adefioye](https://miro.medium.com/fit/c/48/48/1*EHweqnww2uWcqa7d_90mZg.jpeg)](/@temidjoy?source=post_page-----d8cb52c978e1----------------------)
翻译：[talisk](https://github.com/talisk)

![](https://miro.medium.com/max/1766/1*oikrjJQi1b5JTpUM0LsQxw.png)

如果你的确对跨平台开发感兴趣，你会知道一个名为 Flutter 的新框架。这是一个 Google 开发的全新的移动应用 SDK，开发人员可以通过一份代码直接为移动端、Web 和桌面端编写漂亮的原生应用。越来越多的科技公司决定使用 Flutter 框架开发多个平台的应用程序。在过去两年中，技术行业对开发者的需求急剧增加。为了满足这一需求，我们有一个由富有热情的开发人员组成的庞大社区，他们愿意开发世界一流的应用程序，而不会遇到任何编程障碍，这一点至关重要。

在这篇文章中，我会与你分享**四个主要的 Dart 小窍门**，能够帮助你快速上手 Flutter 开发。

# 字符串插值

每种语言都有自己的插入两个或多个单词或字符的方法。在 dart 中，可以将表达式的值放入字符串中，如下所示：

``` dart
int x=6;
int y=2;
String sum = '${x+y}';          // 结果是 8
String subtraction = '${x-y}';  // 结果是 4
String upperCase = '${"hello".toUpperCase()}'; // 结果是 HELLO
String concatXY = '$x$y'; // 结果是 '62'
```

# 方法

Dart 语言是一种面向对象语言（OOL）。在这种语言中，函数属于对象，具有一个类型，Function。这意味着可以将函数分配给变量或作为参数传递给其他函数。有趣的是，你还可以像调用函数一样调用类的实例。太棒了是吧！

``` dart
String fullName() {
    String firstName = "Temidayo";
    String lastName = "Adefioye";
    return '$firstName $lastName'; // 返回 'Temidayo Adefioye'
}
int length(String text) {
    return text.length; // 返回 text 的长度
}
```

上面的函数可以用更简洁的方式重写：

``` dart
    int length(String text) => return text.length; // 返回 text 的长度
```

上述方法适用于函数只包含*一个*表达式的情况。这也被称为速记语法。

# 避空运算符

处理好应用程序开发中的空指针异常非常重要，因为这能让您为用户创建无缝体验。Dart 为处理可能为空的值提供了一些便捷的运算符。一个是 `??=` 赋值运算符，仅当变量当前为空时才赋值：

``` dart
int x; // 任何对象的初始值都为空
x ??=6;
print(x); // 结果是 6

x ??=3;
print(x); // 结果仍然是 6

print(null ?? 10); // 结果是 10。如果不为空，则显示左侧的值，否则显示右侧的值
```

# List 数组

在几乎每种编程语言中，最常见的集合可能是对象的数组或有序集合。请注意，Dart 的数组是 List。

``` dart
var numList = [1,2,3,5,6,7];
var countryList = ["Nigeria","United States","United Kingdom","Ghana","IreLand","Germany"];
String numLength = numList.length; // 结果是 6
String countryLength = countryList.length; // 结果是 6
String countryIndex = countryList[1]; // 结果是 'United //States'
String numIndex = numList[0]; // 结果是 1

countryList.add("Poland"); // 把一个新项目添加到数组中

var emailList = new List(3); // 创建一个固定长度数组
var emailList = new List<String>(); // 数组中实例的类型是 //String
```

一共就这些吗？

![](https://miro.medium.com/freeze/max/30/1*uGJysDvESMupwwDUQJJz0A.gif?q=20)
![](https://miro.medium.com/max/650/1*uGJysDvESMupwwDUQJJz0A.gif)

不！

我为那些对使用 Flutter 和 Dart 开发应用感兴趣的开发者精心策划了一个很棒的备忘清单。

你可以在[这里]](https://github.com/Temidtech/dart-cheat-sheet)查看清单。

在接下来的几周里，清单将更新更多的 Dart 小窍门。

祝你写 Flutter 写得开心~