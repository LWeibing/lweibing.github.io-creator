---
title: "Vue的初识"
date: 2020-06-08T19:02:11+08:00
draft: true
---

## Vue

`Vue` 的作者是尤雨溪,英文名为 Evan You.`Vue` 是一套用于构建用户见面的渐进式框架,主要关注的是 V(视图层),M(数据模型)和 C(控制器)则被简化.

## Vue 项目的搭建

### 使用 @vue/cli

1. 搜索@vue/cil,进入官网
2. 打开文档,打开 创建一个项目 章节
3. 全局安装,在终端运行命令行 `yarn global add @vue/cli`
4. 选择使用的配置
5. 在终端运行命令 `vue create 文件夹名` 即可创建一个 Vue 项目

### CodeSandbox 网站

1. 在浏览器地址栏输入`codesandbox.io`
2. 选择 `vue` 项目,即可创建项目
3. 可以根据自己的需求在网站上进行项目制作
4. 可以选择 File -> Export to ZIP 将文件下载至本地
5. 解压文件压缩包,用 `VScode` 或 `WebStorm` 打开即可

### 自己搭建

1. 使用 `webpack` 或者 `rollup` 进行搭建
2. 这里不做详细介绍,可参考 `https://www.jianshu.com/p/f502380062d1`
3. 该方式不适合新手,适合使用`Vue`半年以上的老手

## 使用 Vue 实例

### 从 HTML 得到视图

使用完整版的`Vue`

- 网页中引入 `vue.js` 或者 `vue.min.js` 文件
  ```javascript
  <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.min.js"></script>
  ```
- 用 `import` 引用 `vue.js` 或者 `vue.min.js` 文件

  ```javascript
  import Demo from "./vue.min.js";
  ```

### 用 JS 构建视图

使用非完整版的`Vue`

- 网页中引入 `vue.runtime.js` 或者 `vue.runtime.min.js`

  ```javascript
  <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.runtime.min.js"></script>
  ```

- 用 `import` 引用 `vue.runtime.js` 或者 `vue.runtime.min.js`

  ```javascript
  import Demo from "./vue.runtime.min.js";
  ```

### 使用 vue-loader

1.  创建一个 .vue 的文件

    ```javascript
    <template>
    <div class="red">
        {{n}}
        <button @click="add">+1</button>
    </div>
    </template>

    <script>
    export default {
    data() {
        return {
        n: 0
        };
    },
    methods: {
        add() {
        this.n += 1;
        }
    }
    };
    </script>
    <style scoped>
    .red {
    color: red;
    }
    </style>
    ```

2.  再将该文件用 `import` 引用

        import Demo from "./Demo.vue";

3.  将.vue 文件翻译成 h 构建方法

    ```javascript
    new Vue({
      el: "#app",
      render(h) {
        return h(Demo);
      },
    });
    ```

注意:

- 使用该方法会导致 `HTML` 只有一个 div#app,SEO 不友好
- 解决:把 `title`, `description`, `keyword`, `h1`, `a` 写好即可

## 完整版和非完整版的区别

|               |           Vue 完整版           |          Vue 非完整版          |                评价                 |
| :-----------: | :----------------------------: | :----------------------------: | :---------------------------------: |
|     特点      |          有 complier           |         没有 complier          |        complier 占 40% 体积         |
|     视图      | 写在 HTML 里或者 template 选项 | 写在 render 函数里,用 h 来创建 |    h 是尤雨溪写好传给 render 的     |
|   cdn 引入    |             vue.js             |         vue.runtime.js         | 文件名不同,生成的换件后缀为 .min.js |
| webpack 引入  |         需要配置 alias         |          默认使用此版          |            尤雨溪配置的             |
| @vue/cli 引入 |          需要额外配置          |          默认使用此版          |         尤雨溪,蒋豪群配置的         |

- 完整版的 `template` 可以写在 `HTML` 里面,也可以在 `JS` 里面,非完整版不支持`template`

  ```javascript
  new Vue({
    el: "#app",
    //完整版用法
    template: `
      <div>{{n}}</div>
    `,
    data: {
      n: 0,
    },
    methods: {
      add() {
        this.n += 1;
      },
    },
  });
  ```

- 非完整版需要用 `render()`,传入一个参数 `h`,并将其返回

  ```javascript
  new Vue({
    el: "#app",
    render(h) {
      // 不完整版用法
      return h("div", [this.n, h("button", { on: { click: this.add } }, "+1")]);
    },
    data: {
      n: 0,
    },
    methods: {
      add() {
        this.n += 1;
      },
    },
  });
  ```

* 完整版有 `complier`,非完整版没有`complier`,体积相比,前者体积比后者体积大`40%`
* 非完整版更加独立,看上去很不方便,但是更为正确的
