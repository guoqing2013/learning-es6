# Generator 函数



## 简介

所谓 Generator，可以把它理解成一个函数的内部状态的遍历器，每调用一次，函数的内部状态发生一次改变（可以理解成发生某些事件）。ES6 引入 Generator 函数，作用就是可以完全控制函数的内部状态的变化，依次遍历这些状态。


**Generator函数的里两个特征**：  
1. function 命令与函数名之间有一个星号  
2. 函数体内部使用 yield 语句，定义遍历器的每个成员，即不同的内部状态

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```


调用Generator函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象。  
下一步，必须调用遍历器对象的`next`方法，使得指针移向下一个状态。  
也就是说，每次调用`next`方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个`yield`语句（或`return`语句）为止。换言之，Generator函数是分段执行的，`yield`语句是暂停执行的标记，而`next`方法可以恢复执行。


```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

每一次当我们调用next()时，generator执行下一个循环迭代然后暂停。

调用Generator函数，返回一个遍历器对象，代表Generator函数的内部指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`语句后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。