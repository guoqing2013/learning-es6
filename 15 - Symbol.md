# Symbol



JS的数据类型：

- Undefined
- Null
- Boolean
- Number 
- String
- Object

`Symbol`是JavaScript的第七种原始类型。

Symbol 是一种特殊的、不可变的数据类型，可以作为对象属性的标识符使用。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

## 语法
`Symbol([description])`

description 
可选的，字符串。可用于调试但不能访问符号本身的符号描述。

如果 Symbol 的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个 Symbol 值。

```javascript
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
```


## 作为属性名的 Symbol


```javascript
var mySymbol = Symbol();

// 第一种写法
var a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
var a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
var a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

## 在对象中查找 Symbol 属性

1. Symbol 作为属性名，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。但是，它也不是私有属性，有一个`Object.getOwnPropertySymbols`方法，可以获取指定对象的所有 Symbol 属性名。

`Object.getOwnPropertySymbols`方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。



2. Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

```javascript
let obj = {
  [Symbol('my_key')]: 1,
  enum: 2,
  nonEnum: 3
};

Reflect.ownKeys(obj)
//  ["enum", "nonEnum", Symbol(my_key)]
```

## Symbol.for()，Symbol.keyFor()


`Symbol.for(key)`接受一个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol值。如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值。

```javascript
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');

s1 === s2 // true
```


`Symbol.keyFor(sym)`方法返回一个已登记的 Symbol 类型值的 key

```javascript
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

Symbol.for()生成新的Symbol会被登记在全局环境中供搜索，而 Symbol()不会。


## 内置的Symbol值


除了定义自己使用的Symbol值以外，ES6还提供了11个内置的Symbol值，指向语言内部使用的方法。

1. Symbol.hasInstance 

一个确定一个构造器对象识别的对象是否为它的实例的方法。使用 instanceof.

1. Symbol.isConcatSpreadable

一个布尔值，表示该对象使用Array.prototype.concat()时，是否可以展开。使用Array.prototype.concat().

1. Symbol.species 
	
1. Symbol.match

1. Symbol.replace

1. Symbol.search

1. Symbol.split

1. Symbol.iterator

1. Symbol.toPrimitive

一个将对象转化为基本数据类型的方法。

1. Symbol.toStringTag

用于对象的默认描述的字符串值。使用Object.prototype.toString().

1. Symbol.unscopables

拥有和继承属性名的一个对象的值被排除在与环境绑定的相关对象外。



### Symbol.iterator 

对象的Symbol.iterator属性，指向该对象的默认遍历器方法。

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
```

对象进行`for...of`循环时，会调用`Symbol.iterator`方法，返回该对象的默认遍历器

```javascript
class Collection {
  *[Symbol.iterator]() {
    let i = 0;
    while(this[i] !== undefined) {
      yield this[i];
      ++i;
    }
  }
}

let myCollection = new Collection();
myCollection[0] = 1;
myCollection[1] = 2;

for(let value of myCollection) {
  console.log(value);
}
// 1
// 2
```