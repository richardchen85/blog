---
title: 浏览器API学习——BOM浏览器对象模型
user: Richard Chen
date: 2019-02-03 14:22:45
updated: 2019-02-03 14:20:20
comments: 1
categories: 浏览器
tags: []
permalink: /post/browser-api-bom.html
---

window 对象是 BOM 的核心，可以控制窗口、框架和弹出窗口，也扮演着 ECMAScript 中 Global 对象的角色。

定义全局变量与在 window 对象上直接定义属性的差别：全局变量不能通过 delete 操作符删除，而定义在 window 对象上的属性是可以的。

<!--more-->

## 窗口和框架

- 窗口切换：top、self、parent
- 框架：window.frames[0] 或者 window.frames['name']

## 系统对话框

alert()、confirm() 和 promot()。

## location 对象

| 属性名   | 例子                                               | 说明                                           |
| -------- | -------------------------------------------------- | ---------------------------------------------- |
| hash     | #contents                                          | 返回 URL 中的 hash 或空字符串                  |
| host     | www.wrox.com:80                                    | 服务器名称和端口号（如果有）                   |
| hostname | www.wrox.com                                       | 不带  端口号的服务器名称                       |
| href     | [http://www.wrox.com/xx/](http://www.wrox.com/xx/) | 返回当前加载的完整 URL，同 location.toString() |
| pathname | /WileyCDA/                                         | 返回 URL 中的目录和（或）文件名                |
| port     | 8080                                               | 端口号或空字符串                               |
| protocol | http:                                              | 返回页面使用的协议                             |
| search   | ?q=javascript                                      | 返回以问号开头的 URL 的查询字符串              |
| origin   | [http://www.wrox.com](http://www.wrox.com)         | 返回 URL 的源信息                              |

每次修改 location 的属性（hash 除外），页面都会以新 URL 重新加载。

### location 的方法

- assign(url)： 打开新 URL 并在  浏览器的历史记录中生成一条记录
- replace(url)：导航到新的 URL，但不生成浏览器记录
- reload(true|false)：为 false 可能从缓存加载，为 true 从服务器重新加载， 调用之后不阻塞后面的代码，所以最好放在代码最后一行

## navigator 对象

| 属性或方法          | 参考值                                                                                                                     |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| appCodeName         | "Mozilla"                                                                                                                  |
| appName             | "Netscape"                                                                                                                 |
| appVersion          | "5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"         |
| connection          | NetworkInformation                                                                                                         |
| cookieEnabled       | true                                                                                                                       |
| doNotTrack          | null                                                                                                                       |
| geolocation         | Geolocation                                                                                                                |
| hardwareConcurrency | 8                                                                                                                          |
| language            | "zh-CN"                                                                                                                    |
| languages           | (3) ["zh-CN", "zh", "en"]                                                                                                  |
| maxTouchPoints      | 0                                                                                                                          |
| mediaCapabilities   | MediaCapabilities                                                                                                          |
| mediaDevices        | MediaDevices                                                                                                               |
| mimeTypes           | MimeTypeArray                                                                                                              |
| onLine              | true                                                                                                                       |
| permissions         | Permissions                                                                                                                |
| platform            | "MacIntel"                                                                                                                 |
| plugins             | PluginArray                                                                                                                |
| presentation        | Presentation                                                                                                               |
| product             | "Gecko"                                                                                                                    |
| productSub          | "20030107"                                                                                                                 |
| userAgent           | "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" |
| vendor              | "Google Inc."                                                                                                              |
| vendorSub           | ""                                                                                                                         |

## screen 对象

| 属性或方法  | 参考值            |
| ----------- | ----------------- |
| availHeight | 971               |
| availLeft   | 0                 |
| availTop    | 23                |
| availWidth  | 1680              |
| colorDepth  | 24                |
| height      | 1050              |
| orientation | ScreenOrientation |
| pixelDepth  | 24                |
| width       | 1680              |

## history 对象

- go(number|url)：在历史记录中  随意跳转，正数向前，负数向后，url 跳转到历史记录中包含该字符串的第一个位置
- back()：后退
- forward()：前进
