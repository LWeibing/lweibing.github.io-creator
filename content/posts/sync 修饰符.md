---
title: "sync 修饰符"
date: 2020-06-12T20:57:58+08:00
draft: false
---

App.vue

```javascript
<template>
  <div class="app">
    App.vue 我现在有 {{total}}
    <hr>
    <Child :money="total" v-on:update:money="total = $event"/>
  </div>
</template>

<script>
import Child from "./Child.vue";
export default {
  data() {
    return { total: 10000 };
  },
  components: { Child: Child }
};
</script>

<style>
.app {
  border: 3px solid red;
  padding: 10px;
}
</style>
```

Child.vue

```javascript
<template>
  <div class="child">
    {{money}}
    <button @click="$emit('update:money', money-100)">
      <span>花钱</span>
    </button>
  </div>
</template>

<script>
export default {
  props: ["money"]
};
</script>


<style>
.child {
  border: 1px solid green;
}
</style>
```

上面的例子可以这样理解,银行里存了 1000 元,支付宝每次点击花钱,就是从银行里扣 100 元钱.我们可以看到,如果没有 `$emit` 和 `$event`,组件里的钱是改变之后,是不会影响到父组件的.

Vue 规则

- 组件不能修改 `props` 外部数据
- `this.$emit` 可以触发事件,并传参
- `$event` 可以获取 `$emit` 的参数

`vue` 修饰符 `sync` 的功能是:当一个子组件改变了一个`props` 的值时,这个变化也会同步到父组件中所绑定.上面的例子也就可以改成如下代码

App.vue

```javascript
<template>
  <div class="app">
    App.vue 我现在有 {{total}}
    <hr>
    <Child :money.sync="total" />
  </div>
</template>

<script>
import Child from "./Child.vue";
export default {
  data() {
    return { total: 10000 };
  },
  components: { Child: Child }
};
</script>

<style>
.app {
  border: 3px solid red;
  padding: 10px;
}
</style>
```

Child.vue

```javascript
<template>
  <div class="child">
    {{money}}
    <button @click="money-100">
      <span>花钱</span>
    </button>
  </div>
</template>

<script>
export default {
  props: ["money"]
};
</script>


<style>
.child {
  border: 1px solid green;
}
</style>
```

可以看出来, `:money.sync="total"` 等价于 `:money="total" v-on:update:money="total = $event"`,就是一个语法糖.
