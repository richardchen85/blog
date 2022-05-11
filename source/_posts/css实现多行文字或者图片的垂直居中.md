---
title: css实现多行文字或者图片的垂直居中
date: 2018-03-08 11:37:46
updated: 2019-02-11 17:33:01
comments: 1
categories: CSS
tags: []
permalink: /post/16.html
---

CSS 实现单行文字在容器垂直居中大家都知道可以用 line-height。如果是多行文字且容器高度不固定倒也简单，直接用 padding 就解决了。我在这里主要说固定高度容器内多行文字的垂直居中，当然是要兼容几个常用浏览器了，其实主要还是 IE6、IE7 了。

<!--more-->

网上有几种方法可以实现，大家可以去搜搜，我这里就不讲解了。我主要说一下我用到的方法，其实都大同小意，也有一点点 css hack 的味道在里面，但不会是什么“\*”号或者是“\_”这种 w3c 不认识的 hack，主要是从 CSS 选择器方面下手。下面来看看：

HTML 代码很简单，我用的是三层容器，主要是为了兼容 IE6、IE7，要不然两层就够了

```html
<div class="container withBorder" lang="zh-cn">
  <div class="vertical-middle">
    <div class="wrap">
      习近平向乌兹别克斯坦政府和人民以诚挚问候和良好祝愿。习近平表示，中乌传统友谊源远流长。古老的丝绸之路早在两千多年前就将两国人民联系在一起。建交21年来，中乌关系全面快速发展。事实证明，中乌世代友好、互利合作是完全正确的战略选择。我期待着同卡里莫夫总统等就发展和深化两个关系以及其他共同关心的国际和地区问题深入交换意见，并签署重要政治文件。我相信，这次访问一定能够取得成功，为中乌战略伙伴使用关系发展注入新的强大动力。
    </div>
  </div>
</div>
```

注意看顶层容器有一个 lang 属性，是的，我就是通过这个属性来 hack 的。因为高级浏览器是支持 display:table 和 display:table-cell 的，可以用这两个属性实现垂直居中，但是 IE7 以下版本的浏览器不支持，但是 IE7 以下浏览器可以使用绝对定位来实现垂直居中，于是有了下面的 CSS 代码：

```css
.withBorder {
  border: 1px green solid;
}
.container {
  width: 500px;
  height: 300px;
  overflow: hidden;
  position: relative;
}
.container:lang(zh-cn) {
  display: table;
  position: static;
}
.container .vertical-middle {
  position: absolute;
  top: 50%;
}
.container:lang(zh-cn) .vertical-middle {
  display: table-cell;
  vertical-align: middle;
  position: static;
}
.container .wrap {
  position: relative;
  top: -50%;
}
```

我们先使用绝对定位的方式来满足 IE7 以下的浏览器实现垂直居中，为什么使用绝对定位然后 top:50%和 top:-50%能居中，而且只能在 IE7 以下版本浏览器中才有效，据说是 IE 的 layout 的 bug，具体原因我也不太清楚，反正是能用就好了，以后再深入去研究。

第二步，也是最关键的，使用:lang 选择器，这个选择器只有 IE8 以上版本和高级浏览器才支持，于是正好，我们可以用这个选择器来实现高级浏览器的垂直居中的 CSS，即 display:table 的方式。

好了，就是这样，附上效果图，效果自己去测试吧。

![css_vertical_align_middle_03](https://github.com/richardchen85/blog/raw/master/static/css_vertical_align_middle_03.png)

本人也是菜鸟，文字表达不太清楚请多包涵，这个方法其实也是借鉴了一些别人的思路。以前总在迫不得已的情况下会使用 hack，现在其实也可以通过选择器来达到同样的效果，虽然也算是 hack，但是总比什么“\*”、“\_”之类的看着好看些，纯属个人偏好了。
