# let

## 基本用法

let声明的变量只在它所在的代码块有效。(块级作用域)

```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

```javascript
for (let i = 0; i < arr.length; i++) {}

console.log(i);
//ReferenceError: i is not defined
```



变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
    a[i] = function () {
        console.log(i);
    };
}
a[6](); // 10
```



变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。
```javascript
var a = [];
for (let i = 0; i < 10; i++) {
    a[i] = function () {
        console.log(i);
    };
}
a[6](); // 6
```




## 块级作用域


ES6允许块级作用域的任意嵌套。

```javascript
{{{{{let insane = 'Hello World'}}}}};
```



块级作用域的出现，实际上使得获得广泛应用的立即执行匿名函数（IIFE）不再必要了。

```javascript
// IIFE写法
(function () {
    var tmp = '';
}());

// 块级作用域写法
{
    let tmp = '';
}
```