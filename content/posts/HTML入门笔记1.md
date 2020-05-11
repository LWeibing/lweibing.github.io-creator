---
title: "HTML入门笔记1"
date: 2020-05-10T15:15:10+08:00
draft: false
---

## 万维网是什么
---
        WWW = URL + HTTP + HTML
WWW( World Wide Web ),中文直译为像世界那么大的网,就是万维网.

## 万维网的诞生
---
在1990年,`Tim Berners-Lee`为了让每个人输入网址就能看到网页,但是当时并没有网址,于是发明了URL,有了网址没有页面就没有什么可以看的,于是就发明了HTML,最后为了完善这个系统化,就有了HTTP的诞生,以此为基础写了第一个浏览器和第一个服务器,并使用浏览器问了服务器,于是`Tim Berners-Lee`成为了"网上冲浪"第一人.

## HTML历史
---
HTML诞生于`Tim Berners-Lee`的一篇叫做 [HTML Tags](http://help.dottoro.com/lhfnwhnn.php) 的文章,最原始的HTML非常简陋,只有18个标签,经过长时间的迭代,如今的标签大约已有110个.

## HTML起手式
---
Emmet感叹号
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

## 章节标签
---
表示文章的层级

|  标签   |     作用     |  标签  |   作用   |
| :-----: | :----------: | :----: | :------: |
|  h1~h6  | 文章各级标题 | footer |   脚部   |
| section |     章节     |  main  | 主要内容 |
| article |     文章     |        | 旁支内容 |
|    p    |     段落     |  div   | 划分区域 |
| header  |     头部     |

## 全局属性
---
所有标签都有的属性

|      属性       |          作用          |
| :-------------: | :--------------------: |
|      class      |    给标签添加类属性    |
|       id        | 给标签添加唯一的id属性 |
| contenteditable | 用户能够对内容进行编辑 |
|     hidden      |          隐藏          |
|      style      |        更改样式        |
|    tabindex     |  可以用 Tab 进行交互   |
|      title      |    鼠标移入提示文字    |

## 内容标签
---
识别内容的结构

|     标签     |         作用          |    标签    |        作用        |
| :----------: | :-------------------: | :--------: | :----------------: |
|   ol + li    |       有序列表        |     a      |        链接        |
|   ul + li    |       无序列表        |     em     | 强调(语气上的强调) |
| dl + dt + dd |   对词汇等进行描述    |   strong   | 重要(内容上的强调) |
|     pre      | 保留空格,回车以及 Tab |    code    |      书写代码      |
|      hr      |        水平线         |     q      |        引用        |
|      br      |         换行          | blockquote |      换行引用      |