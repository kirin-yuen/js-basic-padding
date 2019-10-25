

## 解构赋值
---
解构赋值，顾名思义，**将结构解开赋值给变量**

ES6 允许按照一定的模式，**从数组和对象中提取值，对变量进行赋值**，被称为解构

* 解构可同时使用`默认值`
```js
// 数组解构赋值
// 传统写法
let arr = [1, 2, 3, 4];
let a = arr[0];
let b = arr[1];
let c = arr[2];
let d = arr[3];

console.log(a, b, c, d); // 1,2,3,4

// 解构赋值，按照数组元素顺序，依次赋值给左侧数组里的变量
var [e, f, g, h] = arr;
console.log(e, f, g, h); // 1,2,3,4

var [e, f, g, h, i] = arr;
console.log(i); // undefined 由于数组元素没有第五个值，因此，i 只声明没赋值，所以是 undefined

// 可以使用 es6 的默认值给变量赋默认值
var [e, f, g, h, i="我是默认值"] = arr;
console.log(i); // 我是默认值
```

代码地址 [git repo](https://github.com/kirin-yuen/js-basic-padding.git)
commit 5c9642bfdfdc73be20543b8c39bddd58ce308bec


================分割线================
