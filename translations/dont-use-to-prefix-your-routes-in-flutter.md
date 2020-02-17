---
title: 不要在 Flutter 路由中添加 “/” 前缀！
---

原文作者：[![Derek Lakin](https://miro.medium.com/fit/c/48/48/1*0wz5QRDbtj_-HDgo6fSlrQ.png)](/@dereklakin?source=post_page-----f3844ce1fdd5----------------------)
翻译：[talisk](https://github.com/talisk)

许多应用都有决定首屏展示内容的判断逻辑，比如展示登录页还是用户引导页面。如果你正在使用 **MaterialApp** widget，则通常会设置 **initialRoute** 属性来指定应用启动后期望的页面。（我也倾向于使用 **onGenerateRoute**，如下所示）：

``` dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: '/login',
      onGenerateRoute: generateRoute,
      title: 'Test App',
    );
  }

  static Route<dynamic> generateRoute(RouteSettings settings) {
    switch (settings.name) {
      case '/':
        return MaterialPageRoute(builder: (_) => HomeView());
      case '/login':
        return MaterialPageRoute(builder: (_) => LoginView());
      default:
        return MaterialPageRoute(
          builder: (_) => Scaffold(
            body: Center(
              child: Text('No route defined for ${settings.name}'),
            ),
          ),
        );
    }
  }
}
```

你可能完全没有注意到，特别是如果你的页面性能出色或页面十分轻量的话（或许你已经像我一样踩到坑了！），在这种情况下发生的事情是，推入的第一个路由是 `/`，然后为 `/login` 推送了另一条路由，导致先创建了首页，然后创建了登录页面。

那么，这里发生了什么？好吧，如果您查看 [**initialRoute** property](https://api.flutter.dev/flutter/material/MaterialApp/initialRoute.html) 属性的文档：

> 如果路由包含斜线，则将其视为 “deep link”，并且在推入此路由前，也会推入通往该路由的路由。例如，如果路由为 `/a/b/c`，则该应用将按顺序加载三个路由 `/a`，`/a/b` 和 `/a/b/c`。

该示例未能按照预期表现，如上所述，第一条推入的路由实际上是 `/`。

幸运的是，有一个非常简单的解决方案，那就是从路由中删除 `/` 前缀：

``` dart
initialRoute: 'login',
```

如果你 *需要* 在路径中留着斜杠以进行 “deep link” 或其他操作，则可以随时在 **onGenerateRoute** handler 方法中检查 **settings.isInitialRoute** 的值。对于推送到 **Navigator** 中的第一个路由，值将会是 **true**。

各位撸码快乐哦！