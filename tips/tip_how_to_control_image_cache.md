# 如何控制图片缓存

为了提高图片加载效率，Flutter内部会将加载过的图片，依照最近最少使用原则缓存，这个缓存默认最大100M，默认最多存储1000条。

使用100M的内存来缓存图片，对很多APP来说似乎有些大，所以我们可以设置其大小。方法是：

```dart
PaintingBinding.instance.imageCache.maximumSizeBytes = 10 << 20; // 10MiB，缓存最大10兆，你也可以设置其它值
PaintingBinding.instance.imageCache.maximumSize = 100; // 最多缓存100条
```

同时你也可以通过 `PaintingBinding.instance.imageCache.clear()` 来直接清除当前缓存，比如你当前页面需要很大的图片缓存，而退出当前页面又需要释放的时候。


## 原理

告诉完答案，下面说说为什么要这么干。

我们所有的Image类实例，无论是直接使用  `Image` 还是使用工厂方法 `Image.network` 或 `Image.asset` 等创建，其控制图片的都是其内部的image属性，image属性又是 `ImageProvider` 的子类，`ImageProvider` 通过 `solve` 方法找到图片，并返回 `ImageStream` ，而 `ImageStream` 就代表着图片。

在 `solve` 方法中，调用 `PaintingBinding.instance.imageCache.putIfAbsent()` 方法来获取并缓存图片。

所以我们可以看到，存储在 `PaintingBinding` 单例中的 `ImageCache` 实例，控制着图片缓存。`ImageCache` 里的宏定义 `_kDefaultSize` 和 `_kDefaultSizeBytes` 代表缓存的默认大小。想操作`ImageCache` 的那些实例方法，只有通过 `PaintingBinding.instance `单例才能拿到 `imageCache` 实例。

你可以直接翻看Flutter SDK里面的image_cache.dart源码，里面的方法都很好理解，并且有注释。

