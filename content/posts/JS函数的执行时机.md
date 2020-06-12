---
title: "JS 函数的执行时机"
date: 2020-05-20T20:43:32+08:00
draft: true
---

## 执行时机

时机不同,得到的结果也是不同的,看下面的例子:

```javascript
// 例 1 
let a = 1
function fn(){
    console.log(a)
}
a = 2
fn() // 2
```

```javascript
// 例 2
let a = 1
function fn(){
    console.log(a)
}
fn() // 1
a = 2
```
例 1和例 2,代码其实都差不多,不同的只是 `fn` 的调用时机不同,例 1在代码`a=2`之后调用`fn`,a这个时候已经被重新赋值了,所以得出的结果为2;例 2调用`fn`,结果直接输出为1,后面`a=2`,对其完全没有影响


现在我们知道了时机对函数的影响,我们继续来看下面这个例子:

```javascript
// 例 3
let i = 0
for(i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```
你以为输出的结果会是 `0,1,2,3,4,5` ?仿佛并不是这样,结果是 `6,6,6,6,6,6`,这是为什么呢?

这里我们就要说一下 `setTimeout` ,这个API是指定一个定时器,该定时器在到时候执行里面的函数或代码

```javascript
console.log('执行setTimeout')
    setTimeout(() => {
        console.log('现在是3秒后')
}, 3000)
```
运行上面这段代码,可以看到,当第一句输出后,过了3秒钟,输出了第二句,也就是说 `setTimeout` 可以延迟执行

回到例 3, `setTimeout` 的延迟时间是0,在 `MDN` 中有这样一句话`"如果省略该参数，delay取默认值0，意味着“马上”执行，或者尽快执行。不管是哪种情况，实际的延迟时间可能会比期待的(delay毫秒数) 值长"`

也就是说,不管怎么样,`setTimeout` 都会比浏览器执行普通循环语句用的时间长,异步队列中的6个`setTimeout`执行的时候, `i` 的值已经经过了六次的覆盖,所以打印出来的值就是 `6个6`了


### 那么,我们如何解决呢?

#### 方法一: 

```javascript
// 例 4
for(let i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
} // 0,1,2,3,4,5
```
在ES6中,为了方便使用,以及新手的学习,创造一个新的关键字 `let`,其可以定义一个块级作用域的变量.

在例 4中 `for` 循环中使用 `let` ,作用域只在循环中,而且每次都会创建一个新的 `i`,于是在循环完成后一共有六个 `i` ,异步队列中的6个`setTimeout`执行的时候,将访问到的 `i` 依次输出,结果就是我们想要的结果了.

#### 方法二:

```javascript
let i = 0
for (i = 0; i < 6; i++) {
    times(i)
}
function times(i) {
    setTimeout(() => {
        console.log(i)
    }, 0)
}
```
将 `setTimeout` 从`for`循环中拿出来放入一个函数中,每次`for`循环调用这个函数并将 `i` 传入函数,就算 `setTimeout` 会比较慢,但是和 `for` 循环无关,调用了6个函数也接到了 `i` 的值,当 `setTimeout` 执行的时候,把 `i` 值输出就行了

#### 方法三:

```javascript
let i
for (i = 0; i < 6; i++) {
  const index = i
  setTimeout(() => {
    console.log(index)
  })
}
```
在ES6中,创造一个新的关键字 `const` ,该关键字可以定义一个常量,常量是不可修改的

因此,我们可以用常量去记录每一次循环的 `i` ,在输出这个常量,则可以得到我们想要的结果