---
title: 用FIS3实现组件化及前后端共用模板的尝试
date: 2018-03-10 09:34:58
updated: 2019-02-03 14:20:52
comments: 1
categories: 前端工程化
tags: []
permalink: fis3-share-template-try
---

接触FIS也有段时间了，用得越久越能体会到它的强大。刚开始只是用FIS2+swig解决了作为一个切图仔长久以来模板复用的问题，后来涉及到了一些JS开发，于是用到了我厂的[JDF](https://github.com/putaoshu/jdf)，基本上实现了组件化开发和前后端模板共用，前端模拟velocity语法，因为我厂后台统一使用velocity模板引擎。于是想到在FIS上试试，下面说说详细的尝试过程。
<!--more-->

后端做资源加载的请忽略，作为小前端，没办法推动后端，本文只讨论纯前端资源加载，另外我尝试的方案也许只适合于和我们有相似架构的项目，因为本人对后端了解不深，如果有没想到的或者更好的方案，请告知我一下，非常感谢。

## 先说说模板共用的事儿

> 模板共用最理想的状态，我想应该是前端产出模板后，后端不需要修改，可以直接copy进项目里，这样可以减少重复劳动力，我来试试看能不能做到。

因为后端是velocity模板引擎，幸好以前用gulp时发现了[fool2fish的velocity模块](https://github.com/fool2fish/velocity)，可以满足velocity语法解析，还支持类似于velocity tools的用法。于是想到写一个[fis-parser-velocity](https://github.com/richardchen85/fis-parser-velocity)插件，将velocity模板文件直接生成html。

我喜欢FIS的原因之一就是，像我这样的小白可以很容易写一个parser插件，FIS的插件开发方法请参考[FIS3插件开发](http://fis.baidu.com/fis3/docs/api/dev-plugin.html)。

说回正题，能够解析velocity语法只能满足在开发的时候预览前端效果，要直接把模板拿到后端用还是不行的，请看下面这段前端代码：

``` html
<div class="floor">
  <div class="floor-title">$floorName</div>
  <ul class="floor-item-list">
  #foreach($item in $list)
    <li>名称：$item.name  价格: $item.price</li>
  #end
  </ul>
</div>
```

如果前后端不事先约定好，就可能出现前端变量名叫$floorName和$list，后端却是$loucenName和$dataList，这都还是小事，再看下面前端代码：

``` html
<img src="/path/to/a/picture.jpg">
```

可能图片会存在CDN服务器，所以在生产环境中，可能会是

``` html
<img src="//cdn.xxx.com/0.0.1/path/to/a/picture.jpg">
```

后端拿到我们的模板，要把所有静态资源引用统统加上`//cdn.xxx.com/0.0.1`，后台可以注入一个变量$cdnPath统一在后台修改，但没能达到我想要的效果。

还有一种情况就是资源的合并和指纹识别，比如：

``` html
<link rel="stylesheet" href="/path/to/base.css">
<script src="/path/to/lib/a.js"></script>
<script src="/path/to/lib/b.js"></script>
```

发布后可能会变成

``` html
<link rel="stylesheet" href="/path/to/base_a3c432.css">
<script src="/path/to/lib/pkg_a_b_43xd3f.js"></script>
```

当前端资源文件有修改的时候，你叫后端挨个儿文件改模板，或者你自己去改，可能是会疯掉的。

所以我在fis-parser-velocity插件中加了一个parse选项，如果为true会将vm模板生成html，用于开发时浏览效果，如果为false，则只会加载静态资源交由fis3-postpackager-loader处理，vm语法原样输出，用于提供给后端。

于是得到以下效果：

前端模板文件：index.vm

``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Examples</title>
</head>
<body>
    #parse('widget/header/header.vm')
    <section>
        <h2>this is body</h2>
    </section>
    #parse('widget/footer/footer.vm')
</body>
</html>
```

输出结果文件名依然是：index.vm

``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Examples</title>
    <link rel="stylesheet" href="/widget/header/header_0476b28.css">
    <link rel="stylesheet" href="/widget/footer/footer_20247c7.css">
</head>
<body>
    #parse('widget/header/header.vm')
    <section>
        <h2>this is body</h2>
    </section>
    #parse('widget/footer/footer.vm')
    <script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
    <script src="/widget/header/header_f450bec.js"></script>
    <script src="/widget/footer/footer_a92d4a6.js"></script>
</body>
</html>
```

可以看到，VM语法没做解析，但是被`#parse`文件的静态资源已经引入进来，也做了相应的处理。这就是我简单的组件化的处理方案。

## 再来说说组件化

想起[@民工精髓](http://weibo.com/sharpmaster)大大的一句话：

> 组件化的本质目的并不一定是要为了可复用，而是提升可维护性

至少在我的工作中，能够跨项目复用的组件是很少的，组件化带给我最多的还是易于维护、适合多人协作开发。

看看以下目录结构：

```nohighlight
widget 组件目录
  ├ header
  │  ├ header.vm 模板文件
  │  ├ header.js JS文件
  │  ├ header.scss  css样式
  │  ├ header.mock  模拟数据文件
  └ nav
     ├ nav.vm
     ├ nav.js
     ├ nav.scss
     └ nav.mock
```

在实际开发中，我希望的是在页面中`#parse('widget/header/header.vm')`过后，能够自动将`widget/header/header.js`和`widget/header/header.css`引入页面，
在解析velocity语法时，可以将`widget/header/header.mock`文件作为context数据源。

好在fool2fish的velocity模块中，可以直接获取到某个VM文件的AST树，可以轻松拿到所有通过`#parse`引入的模板文件，再分析该模板的同名js、css、mock文件，如果存在，则加入页面依赖缓存，在非模块化项目中，希望是这样：

``` html
<script src="/widget/a/a.js"></script>
<script src="/widget/b/b.js"></script>
```

在使用模块化框架，如require、seajs时，我希望是这样：

``` js
require(["/widget/a/a.js", "/widget/b/b.js"]);
```

或者

``` js
seajs.use(["/widget/a/a.js", "/widget/b/b.js"]);
```

剩下的交由`fis3-postpackager-loader`去处理。

## 来个总结

其实也没什么好总结的，本来是个内向的人，也很少写文字，所有内容都在fis-parser-velocity插件里面，另外还有个完整的案例在[fiskit](https://github.com/richardchen85/fiskit)封装和[fiskit-demo](https://github.com/richardchen85/fiskit-demo)中可以参考。FIS是个很强大的东西，我也只是用到了其中很小的一点来解决我现有业务上的一些问题，经验尚浅，还有很多东西需要学习。
