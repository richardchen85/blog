---
title:  如何写出切实有效的javascript代码
date: 2018-03-07 23:13:21
updated: 2019-02-11 17:31:09
comments: 1
categories: JavaScript
tags: []
permalink: 15
---

1、使用变量之前先申明它，未使用var申明的变量默认为全局变量，有可能造成冲突。

2、使用分号“;”结束一语句。

<!--more-->

3、避免使用全局变量——用命名空间代替，这样可以避免与其它js库冲突。

4、考虑使用一种变量命名规范，比如匈牙利标记法。

5、避免使用难以阅读和理解的语言结构，比如eval()。

6、尽量使用内置的函数和库，这样你就不需要额外去写代码，另外它们能在浏览器的本地代码得以实现，效率要高很多。

7、匿名函数就是以参数的形式传递的函数或者在行内定义的没有名字的函数。你可以直接定义和分配给任何变量。

8、在循环中善用continue跳过不必要的步骤。

9、记住object也是关联数组，obj.property 和 obj[“property”]的写法是一样的。关联数组的key是被转换成字符串的，所以obj[3+4]和obj[“7”]的写法是一样的。

10、在循环中要注意对象的修改，像数字这样的原始值传递给函数的是它本身的复制品，在函数内修改它的值对它本身没有任何影响。对象和数组却是按引用传递的，如果你在函数内部修改了它的值，也会影响到对象或者数组本身。

11、避免使用”with”语句，用一个临时变量来转存对象。

12、避免使用eval()和Function构造函数，用匿名函数代替。

> Avoid using the eval() and Function constructors

```javascript
function addMethodToObj(oObject, oProperty, sFunctionCode) {
    oObject[oProperty] = new Function(oFunctionCode);
}

addMethodToObj(oMyObj, 'rotate90', 'this.angle=(this.angle+90)%360);
```

> Use and anonymous function instead

```javascript
function addMethodObj(oObject, oProperty, fFunctionCode) {
    oObject[oProperty] = fFunctionCode;
}

addMethodToObj(oMyObj, 'rotate90', function () {
    this.angle = (this.angle + 90) % 360;
});
```

13、不要传递字符串给setTimeout和setInterval，传递函数来代替。

> Don't pass strings to setTimeout() or setInterval()

```javascript
setInterval('displayData()', 1000);
setTimeout('moveElementBy(x);x+=5;', 1000);
```

> Pass functions instead

```javascript
function displayData() { /*...*/ };
setInterval(displayData, 1000);

setTimeout(function () {
    moveElementBy(x);
    x += 5;
});
```

14、用”+=”符号代替连续的”+”号。

> Use string concatenation instead of the "+" operator

```javascript
var sNewStr = "";
sNewStr += "str x " + "str y" + "str z"; // causes temp string creation
```

> Use "+=" string concatenation operator

```javascript
var sNewStr = "";
sNewStr += "str x";
sNewStr += "str y";
sNewStr += "str z";
// ...
```

15、在操作大量HTML标记的时候，使用innerHTML属性代替DOM操作。

浏览器解析HTML代码的效率很高，所以用innerHTML的方式将HTML插入文档的方式比用DOM操作的方式要快。

```javascript
element.innerHTML = "<div><p>This is some text inside a paragraph and a div and being assigned to an element</p></div>";
```

16、避免一次性进行大规模的DOM改变。

改变DOM元素会导致文档流重新解析，可以临时创建一个对象，在这个对象中操作所有的改变，最后将结果插入文档。

```javascript
var oOrignElem = document.getElementById('myDiv');
var oClone = oOrignElem.cloneNode(true);
oClone.setAttribute('style', 'width:50%');

var oNewElem = null, sContent = "";
var cStrs = asStrings.length; // supose this is a big array
for (var i = 0; i < cStrs; i++) {
    oNewElem = document.createElement('p');
    sContent = document.createTextNode(asStrings[i]);
    oNewElem.appendChild(sContent);
    cloned.appendChild(oNewElem);
}
oOrignElem.parentNode.replaceChild(oClone, oOrignElem);
```

17、用命名空间创建可重用的模块，避免模块间的冲突。

JavaScript原生不支持创建模块，所以你需要用到JavaScript的对象来仿冒命名空间。

更多内容将继续更新……
