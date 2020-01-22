# Don’t use ‘/’ to prefix your routes in Flutter!

[![Derek Lakin](https://miro.medium.com/fit/c/48/48/1*0wz5QRDbtj_-HDgo6fSlrQ.png)](/@dereklakin?source=post_page-----f3844ce1fdd5----------------------)

Many applications have conditional logic to determine which page to show first for things such as login, onboarding, etc. If you’re using the **MaterialApp **widget, then you typically set the **initialRoute **property to specify where you want the app to start (I also tend to use **onGenerateRoute** as shown below):
MaterialApp using initialRoute and onGenerateRoute
What you might not notice (or you may have done and been really frustrated by it like me!), particularly if you have performant or lightweight pages, is that what happens in this situation is that the first route pushed is ‘/’, then another route is pushed for ‘/login’ resulting in your home page being created and then your login page.

So, what’s happening here? Well, if you look at the documentation for the [**initialRoute** property](https://api.flutter.dev/flutter/material/MaterialApp/initialRoute.html):

> If the route contains slashes, then it is treated as a “deep link”, and before this route is pushed, the routes leading to this one are pushed also. For example, if the route was `/a/b/c`, then the app would start with the three routes `/a`, `/a/b`, and `/a/b/c` loaded, in that order.

What the example fails to explain properly is that the first route pushed is actually ‘/’ as I described above.

Thankfully, there’s a really simple solution and that’s to remove the ‘/’ prefix from your routes:

``` dart
initialRoute: 'login',
```

If you *need* to keep slashes in your routes for deep linking or something else, then you can always check the value of **settings.isInitialRoute** in your **onGenerateRoute** handler. This will be **true** for the very first route being pushed onto the **Navigator**.

Happy coding!