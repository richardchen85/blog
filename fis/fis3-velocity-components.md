# 用FIS3实现组件化及前后端共用模板的尝试

接触FIS也有段时间了，用得越久越能体会到它的强大。刚开始只是用FIS2+swig解决了作为一个切图仔长久以来模板复用的问题，后来涉及到了一些JS开发，于是用到了我厂的[JDF](https://github.com/putaoshu/jdf)，基本上实现了组件化开发和前后端模板共用，前端模拟velocity语法，因为我厂后台统一使用velocity模板引擎，于是想到在FIS上试试，下面说说详细的尝试过程。

## 先说说模板共用的事儿

> 模板共用最理想的状态，我想应该是前端产出模板后，后端不需要修改，可以直接copy进项目里，这样可以减少重复劳动力，我来试试看能不能做到。

因为后端是velocity模板引擎，幸好以前用gulp时发现了[fool2fish的velocity模块](https://github.com/fool2fish/velocity)，可以满足velocity语法解析，还支持类似于velocity tools的用法。于是想到写一个[fis-parser-velocity](https://github.com/richard-chen-1985/fis-parser-velocity)插件，将velocity模板文件直接生成html。

我喜欢FIS的原因之一就是，像我这样的小白可以很容易写一个parser插件，FIS的插件开发方法请参考[FIS3插件开发](http://fis.baidu.com/fis3/docs/api/dev-plugin.html)。

说回正题，能够解析velocity语法只能满足在开发的时候预览前端效果，要直接把模板拿到后端用还是不行的，请看下面这段前端代码：
```html
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
```html
<img src="/path/to/a/picture.jpg">
```
可能图片会存在CDN服务器，所以在生产环境中，可能会是
```html
<img src="//cdn.xxx.com/0.0.1/path/to/a/picture.jpg">
```
后端拿到我们的模板，要把所有静态资源引用统统加上`//cdn.xxx.com/0.0.1`，后台可以注入一个变量$cdnPath统一在后台修改，但始终还是不方便。
