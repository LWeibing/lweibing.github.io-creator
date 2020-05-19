---
title: "JS 对象基本用法"
date: 2020-05-19T22:51:51+08:00
draft: false
---

## 对象 object
js中唯一一种复杂类型

* 无序的数据集合
* 键值对的集合

### 声明方式
```javascript
let obj = {'name':'Tom','age':18}

let obj = new Object({'name':'Tom'})
```

注意:

* 键名是字符串,不是标识符,可以包含任意字符
* 引号可以省略,省略之后就只能写标识符
* 就算省略了引号,键名还是字符串

### 属性名

所有的属性名会自动变成字符串

```javascript
Object.keys(obj) //可以得到 obj 的所有的 key
```

#### 变量做属性名

```javascript
let p1 = 'name'

let obj = {
    [p1]: 'Tom'
}
```
这样写的话, obj 的属性名为 `'name'`

细节:

* 不加 `[]` 的属性名会自动变为字符串
* 加了 `[]` 的属性名会被当做变量求值

### 隐藏属性

JS中每一个对象都有一个隐藏属性__proto__,这个属性存储着其共有属性组成的对象的地址,这个共有属性组成的对象叫做原型,也就是说,隐藏属性__proto__中存储着原型的地址

### 原型

* 每个对象都有原型

    原型里存着对象的共有属性,比如obj的原型就是一个对象,obj.__proto__存着这个对象的地址,这个对象里面有`toString / constructor / valueOf` 等属性

* 对象的原型也是对象

    对象的原型也有对象,obj={}的原型即为所有对象的原型,这个原型包括对象的共有属性,是对象的根,这个原型也有原型,为`null`,是一个比较特殊的原型

### 删除属性

```javascript
delete obj.xxx

delete obj['xxx']
```

上面的代码都可以删除对象 obj 中的 xxx 属性

### 查看所有属性

```javascript
Object.keys(obj) //查看自身所有属性

console.dir(obj) //查看自身+共有属性

obj.hasOwnProperty('toString') //判读一个属性是自身还是共有的
```

### 查看单个属性

```javascript
  obj['key'] //中括号语法

  obj.key //点语法
```

注意:点语法会误导新人,认为 `key` 不是字符串,实际上 `key` 是一个字符串

```javascript
  obj.name === obj['name']

  obj.name !== obj[name]
```

### 修改或增加属性

#### 修改或增加自身属性

```javascript
  let obj = { name:'Tom' }

  obj.name = 'Tom'

  obj['name'] = 'Tom'

  obj['na' + 'me'] = 'Tom'

  let key = 'name'; obj[key] = 'Tom'
  ```

可以用下面的代码进行批量赋值
```javascript
Object.assign(obj,{age:18,gender: 'man'})
```


#### 修改或增加共有属性

```javascript
obj.__proto__['toString'] = 'xxx'

Object.prototype['toString'] = 'xxx'

//一般来说不要修改原型上的属性,会引起很多问题
```

#### 修改原型

```javascript
obj.__proto__ = common

let obj = Object.create(common)
```

注意:所有用 `__proto__` 的代码都强烈不建议使用

### `name` in obj 和 obj.hasOwnProperty 的区别

属性值为 `undefined` 不等于 不含属性名,即属性值为`undefined` 不能判断对象是否拥有该属性

```javascript
'xxx' in obj === false //不含属性名

'xxx' in obj && obj.xxx === undefined //含有属性名,但属性值为`undefined`
```

