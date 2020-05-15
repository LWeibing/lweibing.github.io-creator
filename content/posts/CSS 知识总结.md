---
title: "CSS知识总结"
date: 2020-05-14T22:15:34+08:00
draft: false
---

### [文档流](#text)
### [盒模型](#box)
### [CSS 布局](#layout)
### [CSS 定位](#position)
### [CSS 动画](#animation)
### [层叠上下文](#stacking)
### [浏览器渲染原理](#render)


## <span id="text">文档流</span>

### 流动方向

  * inline
    
    `inline`元素从左至右,位置不够时,可能会将元素分为两块<br/>宽度由内部的`inline`元素的和决定不能指定宽度<br/>高度由`line-height`间接决定,与`height`无关

  * block

    宽度默认自动计算,可以用`width`指定<br/>高度由内部文档流元素决定,可以设置`height`决定

  * inline-block

    从左至右,位置不够时,不会将元素分为两块<br/>结合两者的特点,可以用`width`指定宽度,也可以用`height`设置高度

注意:

   1. 现在不再区分`内联元素`和`块级元素`,而是有`display`决定.
   2. 千万不要在 `inline元素`中加`block元素`.

### overflow
    
当内容的宽度或者高度大于容器的宽度或高度时,会产生溢出现象.

可用`overflow`属性设置:

* visible:默认值,内容不会被改变,显示在容器外部
* hidden:内容会被裁剪,适应容器的大小,没有滚动条
* scroll:内容会被裁剪,适应容器大小,显示滚动条(无论内容是否超出容器,都会显示滚动条)
* auto:内容会被裁剪,适应容器大小,显示滚动条(判断内容是否超出容器,超出容器就显示滚动条,否则不显示)

### 脱离文档流

当元素使用`float`和`position:absolute | fixed`属性,都会使元素脱离文档流,脱离文档流后,父元素将不再计算其高度

## <span id="box">盒模型</span>

CSS中有两种盒子模型,一种是W3C标准的盒子模型(content-box),另一种是IE标准的(怪异)盒子模型(border-box).

### 标准盒子模型与怪异盒子模型的区别

只有当标准盒子模型与怪异盒子模型同时设置了宽度(width),边框(border)和内边距(padding),才能够用肉眼看到明显的差别

#### 标准盒子模型 (content-box):

标准盒子模型的 `width` 和 `height` 指的是内容 (content) 的宽度和高度,不包括内边距(padding),边框(border)和外边距(margin).

![标准盒子模型](/images/box1.png)

标准盒子模型的的大小 = width(content) + padding + border + margin

#### 怪异盒子模型(border-box):

怪异盒子模型的 `width` 和 `height` 指的是内容 (content),内边距(padding)和边框(border) ,不包括外边距边距

![怪异盒子模型](/images/box2.png)

怪异盒子模型的的大小 = width(content + padding + border) + margin

### 如何使用盒子模型

我们可以通过 `box-sizing` 来设置使用什么盒子模型.

* content-box: 默认值,即是标准盒子模型.

* border-box: 怪异盒子模型,程序员更多的使用这个,可以更好的设置一个元素的宽高.

### 使用案例

#### HTML

```
    <div class="content-box"></div>
    <div class="border-box"></div>
```

#### CSS

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

#### 预览效果

![结果](/images/result.png)

## <span id="layout">CSS 布局</span>

### float布局

现在基本不使用,除非要兼容老版本的IE浏览器,手机上基本不用,因为手机上没有IE.


HTML
```
    <div class="clearfix">
        <div class="div1"></div>
        <div class="div2"></div>
    </div>
```

CSS
```
        .clearfix {
            border: 2px solid green;
        }

        .clearfix::after {
            content: '';
            display: block;
            clear: both;
        }

        .div1 {
            width: 100px;
            height: 100px;
            background-color: red;
            float: left;
        }

        .div2 {
            width: 100px;
            height: 100px;
            background-color: blue;
            float: left;
        }
```
注意:在父元素中加入`clearfix::after`,是为了清除由于子元素进行浮动,导致父元素的高度塌陷的问题

![float布局](/images/float.png)

### flex 布局

`flex`是弹性盒子布局,现在的浏览器大部分都支持

#### container 的属性
  
* display 声明

  ```
  .container{
      display: flex | inline-flex;
  }
  ```

* flex-direction 主轴流动方向

  * row:默认值,从左至右
  * column:从上至下
  * row-reserve:从右至左
  * column-reserve:从下至上
    ```
    .container {
        flex-direction: row | column | row-reverse | column-reverse;
    }
    ```

* flex-wrap 换行方式
  
  * nowrap:默认值,不换行
  * wrap:换行(常用)
  * wrap-reserve:头尾交换的换行(基本不用)

    ```
    .container {
        flex-wrap: nowrap | wrap | wrap-reverse;
    }
    ```

* justify-content 主轴对齐方式

  * flex-start:默认值,起点对齐
  * flex-end:终点对齐
  * center:居中对其
  * space-between:两端对齐,并使各个`item`之间间隔相等
  * space-around:在行内平均分配,各个`item`之间的间隔的一半等于第一个和最后一个`item`到行首和行尾的间隔

    ```
    .container {
        justify-content: flex-start | flex-end | center | space-between | space-around;
    }
    ```

* align-items 次轴对齐方式

  * flex-start:起点对齐
  * flex-end:终点对齐
  * center:居中对齐
  * stretch:默认值,如果项目未设置高度或设为auto,则占满容器高度

    ```
    .container {
        align-items: flex-end | flex-end | center | stretch;
    }
    ```

* align-content 多行对齐方式

  * flex-start:起点对齐
  * flex-end:终点对齐
  * center:居中对齐
  * stretch:默认值,占满整个容器
  * space-between:两端对齐,并使各个`item`之间间隔相等
  * space-around:在行内平均分配,各个`item`之间的间隔的一半等于第一个和最后一个`item`到行首和行尾的间隔

    ```
    .container {
        align-content: flex-start | flex-end | center | stretch | space-between | space-around;
    }
    ```

#### item 的属性

* order 排列顺序
  
  默认为0,按照值的大小排列`item`的顺序
  ```
  .items {
      order: 1;
  }
  ```
* flex-grow 增长系数
  
  默认值为0,负值没有效,值数越大,行内占的比例越大.
  ```
  .items {
      flex-grow: 1;
  }
  ```

* flex-shrink 收缩系数

  默认值为1,负值没有效,值数越大,在宽度大于容器宽度时,在行内占据的比例越小,为0时,不收缩
  ```
  .items {
      flex-shrink: 2;
  }
  ```

* align-self 某个item的布局

  * flex-start:起点对齐
  * flex-end:终点对齐
  * center:居中对齐

  ```
  .items {
      align-self: flex-start | flex-end | center;
  }

  ```

### grid 布局
  grid是一种二维布局,最新的浏览器都支持.

* grid 的声明
  ```
  .container{
      display: grid | inline-grid;
  }
  ```

* grid 的使用

  ```
  .container{
      grid-template-rows: 200px 400px 200px;
      grid-template-columns: 40px 50px auto 50px 40px;
  }
  ```
  上面的代码创建了一个三行五列的`grid`表格.

* grid 的选取

  ```
  .item {
      grid-row-start: 1;
      grid-row-end: 3;
      background-color: red;
        }
  }
  ```
  通多代码,选取了`grid`表格中的从上至下第一条线到第三条线中的格子,并把它们的背景色改为红色

* 份
    
    类似于`flex`中的`flex-grow`.可以指定占据的比例

    ```
    .container {
              grid-template-columns: 1fr 1fr 40px 10% 10em;
          }
    ```

* grid-gap 间隙
  
    指定格与格之间的间隙
    ```
    .container {
        grid-column-gap:12px;
    }
    ```

* grid-template-area 分区

  ```
  .container {
      grid-template-areas:
          "header header header"
          "aside main ad"
          "footer footer footer"
      }
  }
  .container>header{
      grid-area:header
  }
  ```

## <span id="position">CSS 定位</span>

### position

* static

    默认值,在文档流中

* relative

    相对定位,比文档流要高一层,但不脱离文档流.

    作用:

    1. 用于做位移(很少用)
    2. 用来做`absolute`的父元素
    3. 配合`z-index`

* absolute

    绝对定位,脱离原来的位置,另起一层,定位基准是祖先中第一个不是`static`的元素

    作用:

    1.  关闭对话框按钮
    2.  鼠标提示文字
    3.  配合`z-index`

    注意:

    1. 某些浏览器不写上`top`,`left`会导致位置错乱
    2. 善用`left:50%`,加`-margin:width/2`或者`transform:translateX(-50%)`
    3. 一般用了`absolute`都要在父元素中补一个`relative`

* fixed

    固定定位,定位的基准是`viewport(视口)`,但当父元素中使用了`transform:scale`属性,将会出错.

    作用:

      1. 边栏广告
      2. 返回顶部按钮
      3. 配合`z-index`

    注意: 手机上尽量不要使用,很多bug

* sticky

    粘滞定位

    作用: 滑动到顶部时,粘滞在顶部的导航栏

    注意: 兼容性很差

## <span id="animation">CSS 动画</span>

### transform

  * translate(translateX,translateY)  沿X轴和Y轴上位移

    * translateX:沿X轴上位移
    * translateY:沿Y轴上位移
    * translateZ:沿Z轴上位移
    * translate3d(translateX,translateY,translateZ):沿X轴,Y轴和Z轴上位移

  * scale(scaleX,scaleY)  沿X轴和Y轴上缩放

    * scaleX:沿X轴上缩放
    * scaleY:沿Y轴上缩放
    * scaleZ:沿Z轴上缩放
    * scale3d(scaleX,scaleY,scaleZ):沿X轴,Y轴和Z轴上缩放

  * rotate(rotateX,rotateY)  沿X轴和Y轴上旋转

    * rotateX:沿X轴上旋转
    * rotateY:沿Y轴上旋转
    * rotateZ:沿Z轴上旋转
    * rotate3d(rotateX,rotateY,rotateZ):沿X轴,Y轴和Z轴上旋转

  * skew(skewX,skewY)  沿X轴和Y轴上倾斜

    * skewX:沿X轴上倾斜
    * skewY:沿Y轴上倾斜
    * skewZ:沿Z轴上倾斜
    * skew3d(skewX,skewY,skewZ):沿X轴,Y轴和Z轴上倾斜

### transition

  作用: 补充中间帧

  语法:

    transition:property duration timing-function delay;

  过度方式:

    timing-function:ease | ease-in | ease-out | ease-in-out | liner | cubic-bezier(0.1, 0.7, 1.0, 0.1);

  注意: 

  * 可以使用 `all` 代替所有属性 
  * `display:none => block` 没法过渡,一般改为`visibility:hidden => visible` 

### animation

关键帧动画,可以处理多次动画

语法:

    animation: duration | timing-function | delay | iteration-count | direction | fill-mode | play-state | name

属性:

|      属性       |            作用            |
| :-------------: | :------------------------: |
|    duration     |     指定动画周期的时长     |
| timing-function |        动画过渡方式        |
|      delay      |            延迟            |
| iteration-count |          循环次数          |
|    direction    |        是否反向播放        |
|    fill-mode    | 动画执行前后为目标应用样式 |
|   play-state    |       允许暂停和播放       |
|      name       |          动画名称          |


使用示例:

HTML
```
 <div class="box">
        <div class="left"></div>
        <div class="right"></div>
        <div class="bottom"></div>
    </div>
```

CSS
```
* {
            padding: 0;
            margin: 0;
            box-sizing: border-box;
        }

        .box {
            margin: 200px;
            position: relative;
            display: inline-block;
            animation: .5s infinite heart;
        }

        @keyframes heart {
            0% {
                transform: none;
            }

            50% {
                transform: scale(1.5);
            }

            100% {
                transform: none;
            }
        }

        .left {
            width: 100px;
            height: 100px;
            background: red;
            transform: rotate(45deg) translateX(80px);
            position: absolute;
            bottom: 100%;
            right: 100%;
            border-radius: 50%;
            border-radius: 50% 0 0 50%;
        }

        .right {
            width: 100px;
            height: 100px;
            background: red;
            transform: rotate(45deg) translateY(80px);
            position: absolute;
            bottom: 100%;
            left: 100%;
            border-radius: 50% 50% 0 0;
        }

        .bottom {
            width: 100px;
            height: 100px;
            background: red;
            transform: rotate(45deg);
        }
```

效果:
![动画截图](/images/small-heart.png)
![动画截图](/images/big-heart.png)


## <span id="stacking">层叠上下文</span>

 定位元素(z-index=1) > 文字(内联子元素) > 浮动元素 > 块级元素 > 边框(border) > 背景(background) > 定位元素(z-index=-1)  > html

注意:每一个层叠上下文就是一个小世界,只有同一个小世界才能够进行比较,小世界里的z-index与小世界外的z-index无关.

哪些属性可以创建层叠上下文:

* z-index部位auto,并有 relative/absolute/fixed
* opacity 
* transform
* html 元素

## <span id="render" >浏览器渲染原理</span>

### 步骤:
1. 根据`HTML`,构建一个`HTML`树(DOM)
2. 根据`CSS`,构建一个`CSS`树(CSSOM)
3. 将两根树合并,成为一棵渲染树(render tree)
4. Layout 布局(文档流,盒模型,计算大小和位置)
5. Paint 绘制(把边框颜色,文字颜色,阴影等绘制)
6. Compose 合成(根据层叠关系展示画面)

### 如何更新样式:

  一般用`JS`来更新样式

### 有三种不同的渲染方式

1. JS/CSS >> 样式 >> 布局 >> 绘制 >> 合成

    例如:使用`div.remove()`删除元素

2. JS/CSS >> 样式 >> 绘制 >> 合成

    例如:使用`background`改变背景颜色

3. JS/CSS >> 样式 >> 合成

    例如:使用`transform`属性改变样式


