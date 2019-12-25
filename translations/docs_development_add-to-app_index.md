---
title: 将 Flutter 集成到现有应用
description: 将 Flutter 作为 library 集成到现有的 Android 或 iOS 应用。
---

## 集成到现有应用

有时候，用 Flutter 一次性重写整个已有的应用是不切实际的。
对于这些情况，Flutter 可以作为一个库或模块，
集成进现有的应用当中。
模块引入到您的 Android 或 iOS 应用（当前支持的平台）中，
以使用 Flutter 来渲染一部分的 UI，或者仅运行多平台共享的 Dart 代码逻辑。

仅需几步，你就可以将高效而富有表现力的 Flutter 引入您的应用。

在 Flutter v1.12 中，添加到现有应用的基本场景已被支持，
每个应用在同一时间可以集成一个全屏幕的 Flutter 实例。
目前仍有以下限制：

- 运行多个 Flutter 实例，或在屏幕局部上运行 Flutter 可能会导致不可预测的行为；
  
- 在后台模式使用 Flutter 的能力还在开发中；
  
- 将 Flutter 库打包进另一个可共享的库或将多个 Flutter 库打包到同一个应用中，都未被支持。

## 已支持的特性

### 集成到 Android 应用

{% include app-figure.md image="docs_development_add-to-app_index_01.gif" alt="Add-to-app steps on Android" %}

- 在 Gradle 脚本中添加一个自动构建并引入 Flutter 模块的 Flutter SDK 钩子。
  
- 将 Flutter 模块构建为通用的 
  [Android Archive (AAR)](https://developer.android.google.cn/studio/projects/android-library)
  以便集成到您自己的构建系统中，并提高 Jetifier 与 AndroidX 的互操作性；
  
- [FlutterEngine](https://api.flutter-io.cn/javadoc/io/flutter/embedding/engine/FlutterEngine.html) 
API 用于启动并持续地为挂载
 [FlutterActivity](https://api.flutter-io.cn/javadoc/io/flutter/embedding/android/FlutterActivity.html) 或
 [FlutterFragment](https://api.flutter-io.cn/javadoc/io/flutter/embedding/android/FlutterFragment.html)
 提供独立的 Flutter 环境；
  
- Android Studio 的 Android 工程与 Flutter 工程同时编辑，以及 Flutter module 创建与导入向导；
  
- 支持了 Java 和 Kotlin 为宿主的应用程序；
  
- Flutter 模块可以通过使用 [Flutter plugins](https://pub.flutter-io.cn/flutter) 与平台进行交互。
  Android 平台的 plugin 应该[迁移至 V2 plugin API](/docs/development/packages-and-plugins/plugin-api-migration)
  以确保最佳的兼容性。在 Flutter v1.12，大多数 
  [Flutter 团队维护](https://github.com/flutter/plugins/tree/master/packages) 的 plugin，以及 
  [FlutterFire](https://github.com/FirebaseExtended/flutterfire/tree/master/packages) 都已完成迁移；
  
- 支持通过从 IDE 或命令行中使用  `flutter attach`  来实现 Flutter 调试与有状态的热重载。

### 集成到 iOS 应用

{% include app-figure.md image="docs_development_add-to-app_index_02.gif" alt="Add-to-app steps on iOS" %}

- 在 Xcode 的 Build Phase 以及 CocoaPods 中，添加一个自动构建并引入 Flutter 模块的 Flutter SDK 钩子。
  
- 将 Flutter 模块构建为通用的 [iOS Framework](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)
  以便集成到您自己的构建系统中；
  
- [FlutterEngine](https://api.flutter-io.cn/objcdoc/Classes/FlutterEngine.html) API 用于
  启动并持续地为挂载 [FlutterViewController](https://api.flutter-io.cn/objcdoc/Classes/FlutterViewController.html)
  提供独立的 Flutter 环境；
  
- 支持了 Objective-C 和 Swift 为宿主的应用程序；
  
- Flutter 模块可以通过使用 [Flutter plugins](https://pub.flutter-io.cn/flutter) 与平台进行交互；
  
- 支持通过从 IDE 或命令行中使用 `flutter attach` 来实现 Flutter 调试与有状态的热重载。

查看 [add-to-app GitHub 示例仓库](https://github.com/flutter/samples/tree/master/experimental/add_to_app)
中在 iOS 和 Android 平台上引入 Flutter module 的示例项目。 

## 开始

第一步，查看以下工程集成指南

<div class="card-deck mb-8">
  <a class="card" href="https://flutter.cn/docs/development/add-to-app/android/project-setup">
    <div class="card-body">
      <header class="card-title text-center m-0">
        Android
      </header>
    </div>
  </a>
  <a class="card" href="https://flutter.cn/docs/development/add-to-app/ios/project-setup">
    <div class="card-body">
      <header class="card-title text-center m-0">
        iOS
      </header>
    </div>
  </a>
</div>

## API 用法

将 Flutter 集成进您的工程后，可以查看以下 API 使用指南

<div class="card-deck mb-8">
  <a class="card" href="https://flutter.cn/docs/development/add-to-app/android/add-flutter-screen">
    <div class="card-body">
      <header class="card-title text-center m-0">
        Android
      </header>
    </div>
  </a>
  <a class="card" href="https://flutter.cn/docs/development/add-to-app/ios/add-flutter-screen">
    <div class="card-body">
      <header class="card-title text-center m-0">
        iOS
      </header>
    </div>
  </a>
</div>
