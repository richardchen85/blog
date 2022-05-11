---
title: object和embed的wmode属性对布局的影响
date: 2018-03-07 23:41:16
updated: 2019-02-11 17:31:19
comments: 1
categories: CSS
tags: []
permalink: /post/17.html
---

wmode 即窗口模式，总共有三个值，我们来看看 Macromedia 官方的说法：

<!--more-->

### window 模式

默认情况下的显示模式，在这种模式下 flash player 有自己的窗口句柄，这就意味着 flash 影片是存在于 Windows 中的一个显示实例，并且是在浏览器核心显示窗口之上的，所以 flash 只是貌似显示在浏览器中，但这也是 flash 最快最有效率的渲染模式。由于他是独立于浏览器的 HTML 渲染表面，这就导致默认显示方式下 flash 总是会遮住位置与他重合的所有 DHTML 层。

但是大多数苹果电脑浏览器会允许 DHTML 层显示在 flash 之上，但当 flash 影片播放时会出现比较诡异的现象，比如 DHTML 层像被 flash 刮掉一块一样显示异常。

### Opaque 模式

这是一种无窗口模式，在这种情况下 flash player 没有自己的窗口句柄，这就需要浏览器需要告诉 flash player 在浏览器的渲染表面绘制的时间和位置。这时 flash 影片就不会在高于浏览器 HTML 渲染表面而是与其他元素一样在同一个页面上,因此你就可以使用 z-index 值来控制 DHTML 元素是遮盖 flash 或者被遮盖。

### Transparent 模式

透明模式，在这种模式下 flash player 会将 stage 的背景色 alpha 值将为 0 并且只会绘制 stage 上真实可见的对象，同样你也可以使用 z-index 来控制 flash 影片的深度值，但是与 Opaque 模式不同的是这样做会降低 flash 影片的回放效果，而且在 9.0.115 之前的 flash player 版本设置 wmode=”opaque”或”transparent”会导致全屏模式失效。

了解了这几种窗口模式的含义过后 ，我们在写页面的时候就可以根据需要来设置 wmode 的值了，比如你想要 flash 显示在所有内容之上，那么你就将 wmode 设置为 window 模式，如果你还想在 flash 上面显示其它 HTML 的内容，那么你就将 wmode 设置为 Opaque 或者 Transparent，如果你和笔者一样，之前不了解这点的话，你就会很困惑为什么 HTML 元素的 z-index 设置再大的数字，都会被 flash 挡住，现在终于明白了吧。
