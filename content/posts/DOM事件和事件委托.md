---
title: "DOM事件机制和事件委托"
date: 2020-05-26T19:34:38+08:00
draft: false
---

### DOM 事件机制

有三个 `DIV` 元素, `#grandfather` 包含 `#father` 包含 `#child`,并给三个 `DIV` 绑定一个点击事件,输出一段文字,当我们点击最内层的文字,谁的文字会最先打出来呢?

```javascript
<div id="grandfather">
    <div id="father">
        <div id="child">请点击我</div>
    </div>
</div>

<script>
    let grandfather = document.querySelector('#grandfather');
    let father = document.querySelector('#father');
    let child = document.querySelector('#child');
    grandfather.onclick=()=>{
        console.log('grandfather 被点击')
    }
    father.onclick=()=>{
        console.log('father 被点击')
    }
    child.onclick=()=>{
        console.log('child 被点击')
    }
</script>
```

`IE5`认为先执行 `#child` 中的函数的这种 `从内岛外` 的顺序,叫做`事件冒泡`

`网景`认为先执行 `#grandfather` 中的函数的这种 `从外到内` 的顺序,叫做`事件捕获`

于是 `W3C`发布标准: **`首先` 按照 `从外到内` 的顺序找监听函数,`然后` 按照 `从内到外` 的顺序找监听函数,如果找到监听函数就调用,并提供事件信息,没有就跳过**

```
// 事件绑定 API

IE5: child.attachEvent('onclick',fn) //事件冒泡

网景: child.addEventListener('click',fn) //事件捕获

W3C: child.addEventListener('click',fn,bool)

```

`W3C`在 `网景的事件绑定API` 的基础上,添加了第三个参数`bool`,当 `bool` 没有传值或者值为`false`时,就让 `fn` 走`事件冒泡`;当 `bool` 值为 `true` 时,就让 `fn` 走`事件捕获`

注意:

- `事件捕获` 不可取消,但是 `事件冒泡` 可以用`e.stopPropagation()` 中断冒泡.
- `scroll` 不能够取消冒泡,阻止默认事件也没用,要先阻止 `wheel` 和 `touchstart` 的默认动作
- `Bubbles` 表示是否冒泡
- `Cancelable` 表示是否支持开发者取消冒泡

### 事件委托

`事件委托` 利用 DOM 元素`冒泡`的事件机制,把需要绑定在子元素的响应事件委托给父元素,让父元素进行监听.

```
举个例子:

比如有素描课程,班里每个人都需要买素描纸,铅笔等工具.这个时候可以个人去买个人的,但最好的办法就是派班长去帮所有人买,买回来再分给所有人.这样大的数量可以讲价,节省一点钱,而且老板对班长有印象,下次去买还可以便宜点.

例子中的买工具就是一个事件,每个同学是子元素,班长就是父元素,同学委托班长帮忙买工具就是`事件委托`,`事件委托` 可以减少每个子元素事件的注册,大量节省内存,而且当新增子对象时无需再次对其绑定.
```

可以看出 `事件委托` 的好处:

1. 减少每个子元素事件的注册,大量节省内存
2. 新增子对象时无需再次对其绑定

#### 基本实现:

点击 `li` 可以输出对应的 `innerHTML`

HTML:

```javascript
<ul id="ulList">
  <li>li 1</li>
  <li>li 2</li>
  <li>li 3</li>
  <li>li 4</li>
  <li>li 5</li>
  <li>li 6</li>
</ul>
```

JS:

```javascript
on("click", "#ulList", "li", (e) => {
  console.log(e.target.innerHTML);
});

//封装事件委托函数
function on(eventType, element, selector, fn) {
  if (!(element instanceof Element)) {
    element = document.querySelector(element);
  }
  element.addEventListener(eventType, (e) => {
    const t = e.target;
    if (t.matches(selector)) {
      fn(e);
    }
  });
}
```
