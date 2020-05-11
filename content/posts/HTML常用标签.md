---
title: "HTML常用标签"
date: 2020-05-10T23:00:25+08:00
draft: false
---
html 中有大约110个标签,那是不是这么多的标签我们都需要将其强记下来呢?不,我们只需要记住一些常用的 html 标签,那些不常用的标签,当我们碰到了要使用的情况,再去 MDN 查询即可.话不多说,一起来看看吧

# HTML常用标签
## [a 标签](#a1)
## [table 标签](#table1)
## [img 标签](#img1)
## [from 标签](#from1)
## [input 标签](#input1)
## [其他输入标签](#other)

## <a id="a1">a 标签</a>

### a 标签的作用

* 跳转到外部页面
* 跳转到内部锚点
* 跳转到邮箱或电话等

### a 标签的属性

1. herf
2. target
3. download
4. rel = noopener

#### 1. herf 的取值

网址
  
在 a 标签的 `href` 里面,我们可以入以 `http//:` , `https//:` 和 `//`为头的网址,一般我们用 `//` 为主,因为它的兼容性最好.

        <a href="//baidu.com">百度</a>

路径

可以在 `href` 属性中填入路径.

```
<a href="/a/b/c.html">c.html</a>
<a href="a/b/c.html">c.html</a>
<a href="index.html">index.html</a>
<a href="/index.html">index.html</a>
```
注意,如果你写链接的最前面有 `/` 的时候,在预览时需要在终端用 `http-server -c-1` 或者 `percel xxx.html` 来打开网页,而不是双击打开.

```
sb@DESKTOP-00A1L8G ~/Desktop/JRG/11 (master)
λ http-server -c-1
Starting up http-server, serving ./
Available on:
  http://192.168.1.3:8080
  http://127.0.0.1:8080
Hit CTRL-C to stop the server
```

伪协议
* javascript:代码. 当单击是执行代码,如果为空,这什么事情也不发生
* mailto:邮箱. 可以发送邮件
* tel:电话. 可以拨打电话

```
    <a href="javascript:alert(1);">伪协议</a>
    <a href="javascript:;">空的伪协议</a>
    <a href="mailto:xxxxxxxx@qq.com">给我发邮件</a>
    <a href="tel:xxxxxxxxx">给我打电话</a>
```

id

    <a href="#xxx">查看xxx</a>

当 `href` 值等于一个id的时候,点击则会快速到达该id的位置

其他
* 当 `href` 为空值时,点击将会刷新页面;

    <a href="">会刷新页面的a</a>

* 当 `href` 为 `#` 时,点击将会将页面拉到最顶上;


#### 2.target的取值

内置的名字

* _blank:在新的浏览器标签打开页面
 
        <a href="//baidu.com" target="_blank">新标签打开百度</a>
* _parent:在当前页面打开

        <a href="//baidu.com" target="_parent">当前页面打开百度</a>
  
* _top:在上一层的页面中打开

        <iframe src="iframe-target.html" frameborder="0" name="yyy"></iframe>

        在 iframe-target.html 文件中
        <a href="//baidu.com" target="_top">在顶部打开百度</a>

* _self:自己打开页面

        <iframe src="iframe-target.html" frameborder="0" name="yyy"></iframe>

        //在 iframe-target.html 文件中
        <a href="//baidu.com" target="_self">自己打开百度</a>

程序员命名

* window的name

        //没有xxx页面,则新建xxx页面打开,否则在xxx页面打开
        <a href="//baidu.com" target="xxx">用xxx打开百度</a>
        <a href="//google.com" target="xxx">用xxx打开谷歌</a>

* iframe的name
  
        <iframe src="iframe-target.html" frameborder="0" name="yyy"></iframe>
        <a href="//baidu.com" target="yyy">用xxx打开百度</a>
    
#### 3. download

作用:下载超链接目标

        <a href="index.html">

问题:不是所有浏览器都支持,尤其是手机浏览器.

#### 4. rel = noopener

引用简书 https://www.jianshu.com/p/c8319e095474

> 为了防止window.opener被滥用，在使用targrt=_blank时需要加上rel=noopener

>     <a href="www.baidu.com" target="_blank" rel="noopener" >

## <a id="table1">table 标签</a>

### 相关标签

* table: 表格
* thead: 表格的表头
* tbody: 表格的主体
* tfoot: 表格的页脚
* th: 表头单元格
* tr: 表格的行
* td: 表格的列

用法:
```
<table>
    <thead>
        <tr>
            <th>英语</th>
            <th>翻译</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>hyper</td>
            <td>超级</td>
        </tr>
        <tr>
            <td>target</td>
            <td>目标</td>
        </tr>
        <tr>
            <td>blank</td>
            <td>空白</td>
        </tr>
        <tr>
            <td>self</td>
            <td>自己</td>
        </tr>
    </tbody>
</table>
```
注意: 在写表格时可以不用写 `thead` 和 `tfoot`;当你没有或者漏写了 `tbody` 或者 `tr` 标签,浏览器将会帮你补齐.

### 相关样式

* table-layout: 行列布局用的算法
* border-collapse: 表格边框是否合并
* border-spacing: 表格的格与格之间的距离

注意:一般在 `css reset` 中,`border-collapse` 和 `border-spacing`会将其默认样式清除.

```
table {
            border-collapse: collapse;
            border-spacing: 0;
        }
```

## <a id="img1">img 标签</a>

### 作用: 发出`get`请求,展示一张图片

### 属性:
* alt: 当链接图片失败是,显示其内容
* height: 图片的高
* width: 图片的宽
* src: 图片的地址

注意:可以只设置图片的 `height` 或者 只设置 `width`,两个都设置容易造成图片变形,一个合格的前端工程师,永远不能让图片变形.

### 事件:
img事件 `onload`/`onerror` ,监听图片是否加载成功,在加载失败时,可以通过id来挽救.

```
<script>
      img1.onload = function () {
        console.log("图片加载成功");
      };
      img1.onerror = function () {
        console.log("图片加载成功失败");
        img1.src = "/404.png";
      };
    </script>
```

图片响应式:
```
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

img {
    max-width: 100%;
}
```

## <a id="from1">from 标签</a>

### 作用:发送`get`或者`post`请求,刷新页面

### 属性:
* action: 后端给的地址
* autocomplete: 自动填充
* method: 选择使用 `get` 或者 `post` 请求
* target: 提交的目标

### 事件:
`onsubmit`: 当表单确认按钮点击是触发

注意:可以通过 `input的value` 和 `button` 来修改按钮的文字,区别是,`button`里面可以放入其他标签,例如 `img` 而 `input的value` 不行.


## <a id="input1">input 标签</a>

### 作用: 让用户输入内容

### 属性: 
* text: 文本
* color: 颜色
* password: 密码
* radio: 单选
* checkbox: 多选
* file: 文件 ( 添加 `multiple` 可以多选文件 )

### 事件:

* onchange: 内容改变时触发
* onfocus: 获取焦点时触发
* onblur: 失去焦点时触发

## <a id="other">其他输入标签</a>

### select + option

#### 作用: 可创建下拉多选菜单.
```
<select>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
</select>
```
### textarea

#### 作用: 定义多行文本
        <textarea name="texts" id="texts" cols="30" rows="10"></textarea>

可以通过 `style="resize:none;"` 使多行文本不可拖改大小

注意:
* 一般不监听 `input` 的 `click` 事件
* `from`里面的 `input` 要有 `name`
* `from` 里要放一个 `type="submit"` 才能触发`submit` 事件