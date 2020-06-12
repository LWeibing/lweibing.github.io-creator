---
title: "浅析 MVC"
date: 2020-06-04T21:32:02+08:00
draft: false
---

### MVC

`MVC` 全名是Model View Controller是一种架构设计模式,将数据,界面的显示分离开来,当用户更新页面时,不需要重新编写全部代码.相当于把适用于一些任务的"工具"放在一起,当遇到类似的任务时,就可以用这个"工具包",不用再去一个个的寻找,也可以说 `MVC` 是万金油,适用于大部分情况.

`MVC`每个模块可以分为三个对象:

* M - Model (数据模型) 负责操作所有数据
    ```javascript
    const m = {
        data: {
            n: parseInt(localStorage.getItem('n'))
        },
        update(data){
            Object.assign(m.data,data)
            eventBar.trigger('m-updated')
        }
    }
    ```

* V - View (视图) 负责所有 UI 界面
    ```javascript
    const c = {
        init(container) {
            v.init(container)
            v.render(m.data.n)
            c.bindEvent()
            eventBar.on('m-updated',()=>{
                v.render(m.data.n)
            })
        },

        event: {
            "click #add": "add",
            "click #sub": "sub",
            "click #mul": "mul",
            "click #div": "div"
        },
        add() {
            m.update(m.data.n += 1)
        },
        sub() {
            m.update( m.data.n -= 1)
        },
        mul() {
            m.update( m.data.n *= 2)
        },
        div() {
            m.update( m.data.n /= 2)
        },
        bindEvent() {
            for (let key in c.event) {
                const value = c[c.event[key]]
                const spaceIndex = key.indexOf(' ')
                const part1 = key.slice(0, spaceIndex)
                const part2 = key.slice(spaceIndex)
                v.el.on(part1, part2, value)
            }
        }
    }
    ```
    
注意:由于 C 中的业务太少,一般都会将 V C 合二为一

### 抽象思维

#### 最小知识原则

引入一个模块需要引入`js`,于是我们可以将 `css` 引入 `js`,再将`js`引入`html`.
你需要知道的知识越少越好,模块化奠定了这个基础

main.js

```javascript
import "./app1.js";
```

app1.js

```javascript
import "./app1.css";
import $ from "jquery";
```
注意:这样做有一定的代价,在网速较慢的情况下,页面一开始会是一片空白,既没内容也没有样式,可以用菊花,骨架,占位内容或者SSR来解决该问题

#### 以不变应万变

每个模块都可以利用 M + V + C 搞定

但是也有一定的弊端,第一,在需求简单的时候,会多出很多不需要的代码,第二,在遇到特殊情况的时候,做MVC会比较麻烦

#### 表驱动编程

表驱动编程是一种编程模式,作用是消除冗长的逻辑判断,提高了程序的可读性,减少了重复代码,降低了复杂度.

```javascript
event: {
        "click #add": "add",
        "click #sub": "sub",
        "click #mul": "mul",
        "click #div": "div"
    },
bindEvent() {
    for (let key in c.event) {
        const value = c[c.event[key]]
        const spaceIndex = key.indexOf(' ')
        const part1 = key.slice(0, spaceIndex)
        const part2 = key.slice(spaceIndex)
        v.el.on(part1, part2, value)
    }
}
```

#### 事不过三

* 同样的代码写了三遍,就应该抽成一个函数
* 同样的属性写了三遍,就应该做成公用属性
* 同样的原型写了三遍,就应该用继承

注意:有的时候会造成继承层级太深,无法一下看懂代码,可以通过画图,写文档解决

#### view = render(data)

只要改变 data 就可以找到对应的 view ,比起操作DOM对象,操作render简单很多

注意: render 粗狂的渲染比 DOM 操作浪费性能,可以用 虚拟DOM 解决,可以让 render 进行局部更新

#### 俯瞰全局

把所有的对象看成一个点,一个点和一个点怎么通信,一个点和多个点怎么通信,多个点和多个点怎么通信,最终找出一个专用的负责通信,这个点就是 `event bus`

### EventBus

`EventBus`是一种事件发布订阅模式,借助 `EventBus` 我们可以很好的实现组件之间，服务之间，系统之间的解耦以及相互通信的问题。


监听事件: on()
```javascript
eventBar.on('m-updated',()=>{
    v.render(m.data.n)
})
```

触发事件: trigger()
```javascript
eventBar.trigger('m-updated')
```


### 如何理解模块化

将一个复杂的页面或者程序,按照一定的规则(规范)封装成为几个模块或者文件.模块或者文件暴露出一些接口与外部其他模块或文件进行通信,这就是模块化.就像是生产一台手机,有些人负责生产电池,有的复杂手机外壳,有的负责主板等等,最后在统一调往一个地方,再进行拼合.
模块化的好处就是减少代码的重复性,提高代码的重用性,便于代码的维护.
