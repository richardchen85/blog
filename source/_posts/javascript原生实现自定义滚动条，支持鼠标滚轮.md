---
title: javascript原生实现自定义滚动条，支持鼠标滚轮
date: 2018-03-07 23:52:07
updated: 2019-02-11 17:32:54
comments: 1
categories: JavaScript
tags: []
permalink: 21
---

javascript原生实现自定义滚动条，支持鼠标滚轮，自定义滚动条的样式，这是本人写的小封装之一，github：[https://github.com/richardchen85/jScrollBar](https://github.com/richardchen85/jScrollBar)

<!--more-->

### 使用方法

```javascript
jScrollBar({
    elem: document.getElementById("div2"),
    inner: document.getElementById("cont2"),
    dir: "h",
    barStyle: {
        height:10,
        bottom:0,
        background:"#ccc"
    },
    sliderStyle: {
        height:8,
        top:1,
        background:"#333",
        borderRadius: 8
    }
});
```

### 参数说明

* elem: 要添加滚动条的元素
* inner: 要滚动的元素
* dir: 滚动方向[ v|h ]，v:垂直滚动，h：水平滚动，默认为v
* step: 每次滚动步长(px)，默认为40
* barStyle: {} 滚动条的样式
* sliderStyle: {} 滚动滑块的样式
