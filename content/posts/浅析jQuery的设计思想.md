---
title: "浅析jQuery的设计思想"
date: 2020-05-25T22:05:17+08:00
draft: false
---

## 目录

1. 什么是 jQuery
2. jQuery 的实现
3. 获取元素
4. 链式操作
5. 创建元素
6. 移动元素
7. 删改元素

### 什么是 jQuery

`jQuery` 是目前最长寿,也是目前世界使用最广泛的 `javaScript` 函数库,虽然`jQuery`现在已经过时,但很多大型的网站都还在使用着它.

### jQuery 的实现

前面说了 `jQuery` 是 `javaScript` 函数库,那么 `jQuery` 是怎么由 `javaScript` 构建出来的呢?

#### 封装

简单的来说,`封装` 就是把一些零散的代码组成一个组件.就像是电脑主机,CPU,内存条,硬盘,主板,显卡等零散的零件,用机箱将他们封装在一起,用户使用的时候,并不用知道里面是怎样摆放,是怎样运作的.

#### 接口

被封装的东西,需要暴露一些功能给外部,让用户可以通过这些功能与被封装的东西进行交互与通讯,这些功能我们就叫做`接口`.类似于电脑主机中的 USB 接口,耳机孔,电源插口等等.

所以,`jQuery` 是由 `javaScript` 中许许多多的函数,语句等封装起来的一个库,而 `jQuery` 文档中提供的用法,我们称之为 `API` .

接下来,我们来看看 `jQuery` 如何使用

### 获取元素

我们要修改哪个元素,就在哪个元素上对其使用 `jQuery`,因此,我们必须先找到这个元素.

```javascript
$(document); //可以获取整个文档

$("#test"); //可以获取 ID 为 test 的元素

$(".red"); //可以获取整个页面 class 为 red 的元素

$("div.red"); //可以获取 class 为 red 的div元素

$("#test").find(".red"); //可以获取 #test 元素中 class 为 red 的元素

$("#test").parent(); //可以获取 #test 元素的父级元素

$("#test").child(); //可以获取 #test 元素的子级元素

$("#test").sibling(); //可以获取 #test 元素的兄弟元素

$("#test").next(); //可以获取 #test 元素的下一个兄弟元素

$("#test").prev(); //可以获取 #test 元素的上一个兄弟元素

$("#test").each(fn); //可以遍历 #test 元素中的每一个元素,并执行 fn
```

### 链式操作

`jQuery` 获取元素后,不返回对应的元素,而是返回一个对象,称为 `jQuery构造出来的对象` ,这个对象可以操作对应的元素,因此我们可以一直对对应的元素进行操作,并且所有的操作是可以连在一起,以链条的形式写出,我们称之为`链式操作`.

```javascript
//链式操作
$("#test").find(".red").css("color", "red"); //修改#test 元素中 class 为 red 的元素的 color 样式
```

`jQuery` 提供了 `.end()` 方法.可以让结果集退到上一个操作的元素

```javascript
$("#test").find(".red").css("color", "red").end(); // 回到 #test 元素
```

### 创建元素

```javascript
$("<p>123</p>"); // 创建一个 p 元素,但是只存在内存,没有放入页面

$("#test").append("<div>123</div>"); // 在 #test 元素中创建一个 div 元素 并放入页面
```

### 移动元素

```javascript
$("p").before($("div")); // 可以把 p 元素移动到 div 元素的后面

$("p").after($("div")); // 可以把 p 元素移动到 div 元素的前面

$("div").insertBefore($("p")); // 可以把 div 元素移动到 p 元素的前面

$("div").insertAfter($("p")); // 可以把 div 元素移动到 p 元素的后面

$("div").prepend($("p")); // 可以把 p 元素移动到 div 元素中第一个子代的前面

$("p").prependTo($("div")); // 可以把 p 元素移动到 div 元素中第一个子代的前面

$("div").append($("p")); // 可以把 p 元素移动到 div 元素中最后一个子代的后面

$("p").appendTo($("div")); // 可以把 p 元素移动到 div 元素中最后一个子代的后面
```

### 删改元素

#### 删除元素

```javascript
$("#test").remove(); // 可以删除 #test 元素,不保留被删除元素的事件

$("#test").detach(); // 可以删除 #test 元素,保留被删除元素的事件

$("#test").empty(); // 可以清空 #test 元素中的内容
```

#### 修改元素

```javascript
$("#test").text(); // 可以读写 #test 元素的文本内容

$("#test").html(); // 可以读写 #test 元素的HTML内容

$("#test").attr(); // 可以读写 #test 元素指定的属性

$("#test").css(); // 可以读写 #test 元素指定的style

$("#test").addClass(); // 可以读写 #test 元素的 class

$("#test").on("click", fn); // 可以设置 #test 元素绑定事件,并执行fn

$("#test").off("click", fn); // 可以取消 #test 元素绑定事件,并执行fn
```
