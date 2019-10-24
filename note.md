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