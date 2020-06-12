---
title: "Vue数据响应式"
date: 2020-06-09T20:24:22+08:00
draft: false
---

## Vue 数据响应式

"响应式"是指一个物体对外界的刺激做出反应.`Vue数据响应式`是指,当 data 数据改变时,`Vue`会通知使用该数据的代码,那么这是为什么呢?我们看下面的例子

```javascript
//例一
const myData = {
  n: 0,
};
console.log(myData); // {n:0}
let vm = new Vue({
  el: "#app",
  template: `
   <div>{{n}}
   <button @click="add">+1</button>
   </div>
  `,
  data: myData,
  methods: {
    add() {
      this.n += 1;
      console.log(myData); //{n:(...)}
    },
  },
});
```

我们可以看到,当点击按钮后, `myData` 发生了改变,这是为什么呢?再看下面这个例子

```javascript
//例子二
let obj1 = {
  姓: "高",
  名: "圆圆",
  姓名() {
    return this.姓 + this.名;
  },
};
console.log(obj1.姓名()); //高圆圆

let obj2 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
};
console.log(obj2.姓名); //高圆圆

let obj3 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  set 姓名(xxx) {
    this.姓 = xxx[0];
    this.名 = xxx.substring(1);
  },
};
obj3.姓名 = "刘诗诗";
console.log(obj3.姓名); //刘诗诗
console.log(obj3); // {姓名:(...)}
```

例子二和例子一相比,可以看出,在`data` 发生改变后,两个例子都发生了同样的改变,按照例子二推论下来,可以看出来,例子一的`n`同样被放入了 `get` 和 `set`.我们可以得出看到,在`obj3`中,我们可以读和改这个`姓名`这个属性,但是这个属性实际上是不存在的.

#### 那如果我们要添加新的属性怎么办?

`Object.defineProperty(obj,'n',{...})` 可以给对象添加属性 `value`,可以给对象添加 `getter/setter`.

```javascript
//例三
let _xxx = 0;
Object.defineProperty(obj3, "xxx", {
  get() {
    return _xxx;
  },
  set(value) {
    _xxx = value;
  },
});
console.log(obj3);
```

可以发现,`obj3` 多出了`xxx`这个不存在的属性.

知道了上面这些理论和方法之后,我们现在来了解一下,为什么 `data` 会产生变化

```javascript
let myData5 = {n:0}
let data5 = proxy2({ data:myData5 })
function proxy2({data}){
  let value = data.n
  Object.defineProperty(data, 'n', {
    get(){
      return value
    },
    set(newValue){
      if(newValue<0)return
      value = newValue
    }
  })
```

经过一系列的推理,可以得出上面的这串代码,可以看到`let data5 = proxy2({ data:myData5 })`和`let vm = new Vue({data:myData})` 很相似,所以我们可以知道,当`let vm = new Vue({data:myData})` 就是让 `vm` 成为 `myData` 的代理,同时对 `myData` 上的所有属性进行监控,这是为了防止其变化了,`vm` 却不知道.

所以当 `vm` 得到属性变化的通知时,就会 render(data)

## Vue 中的 bug

### Object.defineProperty 的问题

当我们使用 `Object.defineProperty(obj,'n',{...})` 方法时,必须得先设置一个 `'n'` 这样才能够进行 `监听和代理` ,如果一开始并没有给出 `n` 怎么办?

```javascript
// bug 1
new Vue({
  data: {},
  template: `
    <div>{{n}}</div>
  `,
}).$mount("#app");
```

```javascript
// bug 2
new Vue({
  data: {
    obj: {
      a: 0, // obj.a 会被 Vue 监听 & 代理
    },
  },
  template: `
    <div>
      {{obj.b}}
      <button @click="setB">set b</button>
    </div>
  `,
  methods: {
    setB() {
      this.obj.b = 1; //请问，页面中会显示 1 吗？
    },
  },
}).$mount("#app");
```

Vue 没办法监听一开始不存在的 obj.b

解决办法:

1. 一开始就把 `key` 都声明好,后面不再加入属性
2. 使用 `Vue.set` 或者 `this.$set`

#### `Vue.set` 和 `this.$set`

作用:

- 新增 `key`
- 自动创建代理和监听(如果没有创建过)
- 触发 UI 更新(但不会立即触发)

### data 中有数组

没有办法提前声明所有`key`

```javascript
new Vue({
  data: {
    array: ["a", "b", "c"],
  },
  template: `
    <div>
      {{array}}
      <button @click="setD">set d</button>
    </div>
  `,
  methods: {
    setD() {
      this.array[3] = "d"; //请问，页面中会显示 'd' 吗？
      // 等下，你为什么不用 this.array.push('d')
    },
  },
}).$mount("#app");
```

解决方法:尤雨溪篡改了 7 个数组的 API,可以进行数组的增删,这 7 个 API 会自动处理监听和代理,并更新 UI

## 我对数据格式化的理解

通过上面知识的学习和理解,我认为数据格式化就像是人的非条件反射,当人手指碰到刺痛,灼热等,手部的感觉神经就会通知神经中枢,再由神经中枢发送响应给手部的运动神经,人就会立刻缩回手;在 Vue 中,当监听到 data 发生变化时,Vue 会通知 vm ,再由 vm 进行 render(data).
