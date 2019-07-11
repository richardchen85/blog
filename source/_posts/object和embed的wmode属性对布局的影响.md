---
title: object和embed的wmode属性对布局的影响
date: 2018-03-07 23:41:16
updated: 2019-02-11 17:31:19
comments: 1
categories: CSS
tags: []
permalink: 17
---

wmode即窗口模式，总共有三个值，我们来看看Macromedia官方的说法：

<!--more-->

### window 模式

默认情况下的显示模式，在这种模式下flash player有自己的窗口句柄，这就意味着flash影片是存在于Windows中的一个显示实例，并且是在浏览器核心显示窗口之上的，所以flash只是貌似显示在浏览器中，但这也是flash最快最有效率的渲染模式。由于他是独立于浏览器的HTML渲染表面，这就导致默认显示方式下flash总是会遮住位置与他重合的所有DHTML层。

但是大多数苹果电脑浏览器会允许DHTML层显示在flash之上，但当flash影片播放时会出现比较诡异的现象，比如DHTML层像被flash刮掉一块一样显示异常。

### Opaque 模式

这是一种无窗口模式，在这种情况下flash player没有自己的窗口句柄，这就需要浏览器需要告诉flash player在浏览器的渲染表面绘制的时间和位置。这时flash影片就不会在高于浏览器HTML渲染表面而是与其他元素一样在同一个页面上,因此你就可以使用z-index值来控制DHTML元素是遮盖flash或者被遮盖。

### Transparent 模式

透明模式，在这种模式下flash player会将stage的背景色alpha值将为0并且只会绘制stage上真实可见的对象，同样你也可以使用z-index来控制flash影片的深度值，但是与Opaque模式不同的是这样做会降低flash影片的回放效果，而且在9.0.115之前的flash player版本设置wmode=”opaque”或”transparent”会导致全屏模式失效。

了解了这几种窗口模式的含义过后 ，我们在写页面的时候就可以根据需要来设置wmode的值了，比如你想要flash显示在所有内容之上，那么你就将wmode设置为window模式，如果你还想在flash上面显示其它HTML的内容，那么你就将wmode设置为Opaque或者Transparent，如果你和笔者一样，之前不了解这点的话，你就会很困惑为什么HTML元素的z-index设置再大的数字，都会被flash挡住，现在终于明白了吧。
