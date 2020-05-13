---
title: "CSS盒模型"
date: 2020-05-12T22:15:34+08:00
draft: false
---

CSS中有两种盒子模型,一种是W3C标准的盒子模型(content-box),另一种是IE标准的(怪异)盒子模型(border-box).

## 标准盒子模型与怪异盒子模型的区别

只有当标准盒子模型与怪异盒子模型同时设置了宽度(width),边框(border)和内边距(padding),才能够用肉眼看到明显的差别

### 标准盒子模型 (content-box):

标准盒子模型的 `width` 和 `height` 指的是内容 (content) 的宽度和高度,不包括内边距(padding),边框(border)和外边距(margin).

![标准盒子模型](/images/box1.png)

标准盒子模型的的大小 = width(content) + padding + border + margin

### 怪异盒子模型(border-box):

怪异盒子模型的 `width` 和 `height` 指的是内容 (content),内边距(padding)和边框(border) ,不包括外边距边距

![怪异盒子模型](/images/box2.png)

怪异盒子模型的的大小 = width(content + padding + border) + margin

## 如何使用盒子模型

我们可以通过 `box-sizing` 来设置使用什么盒子模型.

* content-box: 默认值,即是标准盒子模型.

* border-box: 怪异盒子模型,程序员更多的使用这个,可以更好的设置一个元素的宽高.

## 使用案例

### HTML

```
    <div class="content-box"></div>
    <div class="border-box"></div>
```

### CSS

```
div {
    width: 200px;
    height: 100px;
    border: 5px solid red;
    padding: 30px;
    margin: 30px;
}

.content-box {
    box-sizing: content-box;
}

.border-box {
    box-sizing: border-box;
}
```

### 预览效果

![结果](/images/result.png)


