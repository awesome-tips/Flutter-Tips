Build Modes in Flutter
======================

[![JAY TILLU](https://miro.medium.com/fit/c/96/96/2*li4M13D8csNZa-aV6jWloQ.jpeg)](/@jaytillu?source=post_page-----b44f179ad718----------------------)

[JAY TILLU](/@jaytillu?source=post_page-----b44f179ad718----------------------)

![](https://miro.medium.com/max/60/1*-c-1Cr0fgC6RV8AfUH6fRA.png?q=20)

<img class="ds t u gf ak" src="https://miro.medium.com/max/4000/1\*-c-1Cr0fgC6RV8AfUH6fRA.png" width="2000" height="1000" role="presentation"/>

To master a framework, we need to understand its internal working methods. We have to get the answer of **How Things Happening** and **Why That Happening.** Just like a beautiful relationship, _The more you know each other, the more you will understand each other_. So let’s try to know some inner details about flutter and its build modes.

In this article, we will cover

*   What is the build modes?
*   How many build modes are there in the flutter?
*   Why we use them?

# What is Build Modes?

> Based on different developement phase, framework compiles your code in different manner or we can say that in different mode, that mode is called Build Mode…

As we all know that after a certain point of time it’s really hard and time-consuming to maintain the code and make changes. If you ask any Mobile App Developer about App compiling time, then they will tell you the pain. If you have a gigantic app like Facebook, Instagram, Uber or WhatsApp, then the time is far bigger, even the smallest color correction takes **hours** to reflect. And if you are on Android then the situation is far worse.

But here flutter comes into play to get it down. Let me clear up how?

Flutter team redesigns the entire development process, keeping in mind lightning fast compilation speed. When you hit the compile button, while other frameworks recompile your whole code, even if you have the smallest change, which takes a lot of time. But Flutter doesn’t do that.

Instead of compiling the whole code again and again. Flutter team divides the compilation phase into three modes. First mode when you are developing the app, second mode when you are testing the performance of the app and third and the last mode is when you release the app. By dividing the whole compilation process in different modes, flutter ables to reflects lightning-fast changes. Let’s take a look at all of them.

# How many build modes are there and when to use which one?

*   Basically flutter have three build modes:

1.  Debug Mode
2.  Profile Mode
3.  Release Mode

# Debug Mode

> Debug mode is designed to deliver you fast changes, but the app size is big and performance is minimal. So both app size and performance should not judge.

*   In the app development, the first phase is your development phase. In this phase developers make a lot of changes, fixes some bugs and changing some here and there things. So in this phase developers needs to see their changes as soon as possible.
*   So in debug mode, flutter optimizes the app to reflect changes faster.
*   Here dart language comes into play to help flutter to reflect changes faster. Dart code can run via multiple methods. In debug mode, flutter runs your code in a virtual machine. And when you change anything and push hot reload, flutter just injects your changes to the app, without recompiling it.
*   Just like in web-development when you change anything you just hit refresh and your browser reflects that change instantly, the same concept goes here. Your app is running and as you change anything flutter will push that change into your app.
*   As flutter is not recompiling whole code, it just injects the changes, you see your changes instantly. Which helps in faster development and increases your productivity as well.
*   But as your app runs in a virtual machine, the performance is minimal. So when your app is in debug mode you should feel like its slow. But as it’s optimized for faster changes reflection, you should not judge the performance of your app in debug mode.
*   Also while in debug mode the size of the app can seem pretty big. But don’t judge it as well. As its in debug mode…

**Also, when you are in debug mode consider some important things.**

*   [Assertions](https://dart.dev/guides/language/language-tour#assert) are enabled.
*   Service extensions are enabled.
*   Debugging is enabled. (Here you can use [DevTools](https://flutter.dev/docs/development/tools/devtools/overview)).
*   You can debug your app on a physical device, Emulator or Simulator.

# Profile Mode

> Profile Mode is designed to analyze performance of your app while testing.

*   While compiling your app in Profile Mode, flutter assumes that you want to examine your app’s performance. So its completely optimizes it to provide performance as fast as it can.
*   Here your app is ready and you can analyze the real performance of your app.

**Also, when you are in Profile mode consider some important things.**

*   In profile mode, you cannot run your app on Emulator or Simulator. As they are not optimized to do that. So profile mode is disabled for Emulator and Simulator.
*   Tracing is enabled.
*   Some service extensions to analyze performance are enabled.
*   Tooling support for DevTools is also enabled.

**_Flutter recommends using_** [**_DevTools_**](https://flutter.dev/docs/development/tools/devtools) **_to analyze your app’s performance._**

# Release Mode

> Released Mode designed to get your app ready for the Play Store and App Store. It designed to provide faster startups, fast execution, and minimal in size.

*   Now as development and testing are done. It’s time to compile your app for release mode. Here flutter compiles your code to native machine code, via the AOT compilation process, so now your app will pretty fast. for more information on the compilation process [click here](/flutter-widgets/3-flutter-compilation-process-e602d939e147).
*   While compiling the app in release mode flutter assumes that your app is ready for release so optimization is maximum.
*   In release mode, the compilation is optimized for fast startups, fast execution, and minimum app size.

**Also, when you are in Released mode consider some important things.**

*   Assertions are disabled.
*   Debugging information is stripped out.
*   Debugging is disabled.
*   Service extensions are also disabled.
*   Release mode builds don’t support Emulator and Simulator.

# For More Information Please Visit Following Links

*   [Fuchsia OS official site](https://fuchsia.dev/)
*   [Dart official site](https://dart.dev/)
*   [Flutter official site](https://flutter.dev/)

> So that’s it for build modes guys. I hope it helped you. Feel free to let me know If I miss something. Till then Keep Loving, Keep Coding.
> 
> Wanna get in touch with me? Here are links. I’ll love to become your friend. 😊
> 
> [Twitter](https://twitter.com/jay_tillu)
> 
> [Facebook](https://www.facebook.com/jaytillu.1314/)
> 
> [Instagram](https://www.instagram.com/jay.tillu/)
> 
> or just mail me at developerj13@gmail.com
