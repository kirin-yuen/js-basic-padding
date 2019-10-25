js 我不知道的东西太特么多了，基础不扎实，因此这系列是在日常学习中看到的知识点然后自己作不同的尝试去`扩展与填充自己知识空白部分`

## 从 ES6 声明变量的方式开始
---
声明变量这个东西必须用，但真的了解多少？

### 未声明变量
当我直接输出一个`未声明的变量 myname` 是会报一个**未捕获的错误** `Uncaught ReferenceError: myname is not defined`
```js
var log = '能够执行到这里？';
console.log(myname); // Uncaught ReferenceError: myname is not defined
console.log(log); // 上行代码报错后此行及之后的代码不会继续执行
```
![未捕获的错误](./images/0.png)


#### 捕获异常
为了不影响后续代码执行，我们需要用 `try catch` 来捕获错误，

```js
// var 的变量提升
var log = '能够执行到这里？'

try {
    console.log(myname);
    alert('不会执行') // 上行代码捕获到异常后此行代码也不会执行
} catch (e) {
    console.log(e); // 日志输出
    console.warn(e); // 警告黄色输出
    console.error(e); // 错误红色输出
    console.table(e); // 表格的方式输出，包含了错误在文件中第几行第几列
}
console.log(log); // 能够执行到这里？ 由于捕获了异常所以可以执行这行代码
```

这里正好用控制台的 4 个 api 来输出错误信息

![控制台的 4 个 api 来输出错误信息](./images/1.png)

这里你会看到 `try catch` 后的两点变化
* 代码能继续执行
* **未捕获的错误** `Uncaught ReferenceError` 已经变成 `ReferenceError` 


> 这里有个小插曲，为什么变量要用 `myname` 而不是 `name`，因为 name 是 window 的一个内置属性，值为`空字符串`，所以在未声明 name 的前访问是不会报错，但未声明 myName 则胡报错
```js
console.log(name); // ''
console.log(myName); // Uncaught ReferenceError: myName is not defined
```

代码地址 [git repo](https://github.com/kirin-yuen/js-basic-padding.git)
commit 5c9642bfdfdc73be20543b8c39bddd58ce308bec


================分割线================

### var 声明变量
* 变量提升
* 可重复声明并赋值
```js
// var 声明变量
console.log(log); // var 声明的变量会变量提升，因此这里不会报错而是输出默认值 undefined
var log = '存在'
console.log(log); // 存在
var log = "已成功改变"; // 可重复声明 var，不会报错
console.log(log); // 已成功改变
var log; // 不赋值不改变值
console.log(log); // 已成功改变
```

### let 声明变量
* **变量不会提升**
* **不可重复声明并赋值**
```js
// var 声明变量
console.log(log); // var 声明的变量会变量提升，因此这里不会报错而是输出默认值 undefined
var log = '存在'
console.log(log); // 存在
var log = "已成功改变"; // 可重复声明 var，不会报错
console.log(log); // 已成功改变
var log; // 不赋值不改变值
console.log(log); // 已成功改变

// let 声明变量
console.log(log1); // 不能在用 let 声明变量前访问 Uncaught ReferenceError: Cannot access 'log1' before initialization，let 声明的变量不存在提升
let log1 = 'let 声明的变量'; 

let log = '修改 var 变量的值'; // 不可重复声明 var 或 let 声明过的变量 Uncaught SyntaxError: Identifier 'log' has already been declared， SyntaxError 会导致整个程序不运行

// 无论是 var 还是 let 声明过的变量，都不能重复声明
let log1 = '重新赋值'; // Uncaught SyntaxError: Identifier 'log1' has already been declared
let log = '重新赋值'; // Uncaught SyntaxError: Identifier 'log' has already been declared
```

进入**语法分析**时:
* 如果**语法错误**`SyntaxError`，则整个程序不会运行;
* 如果**引用错误**`ReferenceError`，只会导致错误后面的代码无法执行;

代码地址 [git repo](https://github.com/kirin-yuen/js-basic-padding.git)
commit f75661ecbff1687aa05df88ed06961a22d309d21

================分割线================

### 作用域
* js 里面只有`全局作用域`和`函数作用域`，没有局部作用域
* js 运行会有一个预解析过程，解析后会有一个语法树，再进入执行阶段
```js
if (false) {
    var log = "能否输出？"
}
console.log(log); // undefined 这里会有一个预解析（解析后会有一个语法树，再进入执行阶段）的过程，var 声明的变量会在执行前将变量提升到作用域上

if (false) {
    let log1 = "能否输出？" // let 声明的变量只存在离他最近的大括号内
}
console.log(log1); // Uncaught ReferenceError: log1 is not defined，即使是 true log1 这行还是会报错
```
* for 循坏
  * var 声明的变量没有局部作用域，因此控制台可以输出最后计算的值
  * let 声明的变量在 for 循环中使用是存在**局部作用域（离他最近的大括号内）**，因此这里不会覆盖 var 声明的 i，也不会出现重复声明 i
```js
// var 声明的变量没有局部作用域，因此控制台可以输出最后计算的值
for (var i = 0; i < 3; i++) {}
console.log(i); // 3

// let 声明的变量在 for 循环中使用是存在局部作用域（离他最近的大括号内），因此这里不会覆盖 var 声明的 i，也不会出现重复声明 i
for (let i = 0; i < 5; i++) {
    console.log(i); // 1, 2, 3, 4, 5，这里输出的是 let 里的 i
}
console.log(i); // 3 这里输出的 i 是 var 声明的 i
```

* 闭包
  * 使用 var 声明 for 循环的初始变量 j，最终 j 点击按钮的时候会输出计算后的最终值
  * 使用 let 声明 for 循环的初始变量 j，由于**存在块级别作用域，会将 j 的值锁死在当前作用域内**，因此点击会输出对应的 j 值
```js
// 使用 var 声明 for 循环的初始变量 j，最终 j 点击按钮的时候会输出计算后的最终值
for (var j = 0; j < 2; j++) {
    document.querySelector('#var').addEventListener('click', () => console.log(j))
}
// var 声明的也可使用闭包解决没全局作用域的问题
for (var k = 0; k < 2; k++) {
    (function(param){
        document.querySelector('#closure').addEventListener('click', () => console.log(param))
    })(k)
}
// 使用 let 声明 for 循环的初始变量 j，由于存在块级别作用域，会将 j 的值锁死在当前作用域内，因此点击会输出对应的 j 值
// let 的写法明显优雅与闭包，但 let 声明变量性能会稍逊于 var，因为编译时会自动生成闭包
for (let j = 0; j < 2; j++) {
    document.querySelector('#let').addEventListener('click', () => console.log(j))
}
```

**let 的写法明显优雅与闭包，但 let 声明变量性能会稍逊于 var，因为编译时会自动生成闭包**

代码地址 [git repo](https://github.com/kirin-yuen/js-basic-padding.git)
commit 3e68005f3f08cdc5249be9c1ebeff82e95bf23ea

================分割线================

### const 声明常量
js 一直以来都没有常量这个概念，es6 加入了常量，让使用 const 声明的常量变量**不能重新赋值**，多用于引入模块保存到常量
```js
// 常量
const log = '常量会被改变吗？';
log = '不会'; // Uncaught TypeError: Assignment to constant variable.
console.log(log);
```

代码地址 [git repo](https://github.com/kirin-yuen/js-basic-padding.git)
commit e6dbf861265b7b2901e1acd97fa1fdd1cbdbce2b