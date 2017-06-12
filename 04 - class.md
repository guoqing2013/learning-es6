# Class

## 一、Class基本语法

### 概述

(1) JavaScript语言的传统方法是通过构造函数，定义并生成新对象。
```javascript
function Point(x,y){
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};
```


(2) ES6 定义类
```javascript
class Point {
  constructor(x, y) { 
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  } 
}
```

`this`关键字则代表实例对象

ES6类的所有方法(上例的constructor、toString)都定义在类的prototype属性上面。


(3) 使用`Object.assign`方法可以一次向类添加多个方法。
```javascript
class Point {
	constructor(){
		//...
	}
}

Object.assign(Point.prototype, {
	toString(){},
	toValue(){}
});
```


(4) ES6的类，完全可以看作构造函数的另一种写法。
```javascript
class Point{
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```


(5) 类的内部所有定义的方法，都是不可枚举的（non-enumerable）
```javascript
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype) // []
Object.getOwnPropertyNames(Point.prototype) // ["constructor","toString"]
```


(6) 类的属性名，可以采用表达式
```javascript
let methodName = "getArea";
class Square{
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

### constructor 方法

constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。

```javascript
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo
// false
```




### name属性

`name`属性总是返回紧跟在`class`关键字后面的类名。

```javascript
class Point {}
Point.name // "Point"
```


### Class表达式

使用表达式定义类。

```javascript
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```

这个类的名字是`MyClass`，而不是`Me`，Me只在Class的内部代码可用，指代当前类。

```javascript
let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined
```


如果Class内部没用到的话，可以省略Me，也就是可以写成下面的形式。

```javascript
const MyClass = class { /* ... */ };
```




## 二、Class的继承

### 通过extends关键字实现继承

(1) Class之间可以通过`extends`关键字实现继承

```javascript
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

上面代码中，`constructor`方法和`toString`方法之中，都出现了`super`关键字，它在这里表示父类的构造函数，用来新建父类的`this`对象。


(2) 子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。这是因为子类没有自己的`this`对象，而是继承父类的`this`对象，然后对其进行加工。如果不调用`super`方法，子类就得不到`this`对象。

```javascript
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

let cp = new ColorPoint(); // ReferenceError
```


(3) 如果子类没有定义constructor方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何一个子类都有constructor方法。

```javascript
constructor(...args) {
  super(...args);
}
```

### super关键字

`super`这个关键字，有两种用法，含义不同。

（1）作为函数调用时（即`super(...args)`），`super`代表父类的构造函数。

（2）作为对象调用时（即`super.prop`或`super.method()`），`super`代表父类。注意，此时`super`即可以引用父类实例的属性和方法，也可以引用父类的静态方法。

```javascript
class B extends A {
  get m() {
    return this._p * super._p;
  }
  set m() {
    throw new Error('该属性只读');
  }
}
```

上面代码中，子类通过`super`关键字，调用父类实例的`_p`属性。


### 原生构造函数的继承

原生构造函数是指语言内置的构造函数，通常用来生成数据结构。ECMAScript的原生构造函数大致有下面这些。

- Boolean()
- Number()
- String()
- Array()
- Date()
- Function()
- RegExp()
- Error()
- Object()

ES6允许继承原生构造函数定义子类，因为ES6是先新建父类的实例对象`this`，然后再用子类的构造函数修饰`this`，使得父类的所有行为都可以继承。下面是一个继承`Array`的例子。

```javascript
class MyArray extends Array {
  constructor(...args) {
    super(...args);
  }
}

var arr = new MyArray();
arr[0] = 12;
arr.length // 1

arr.length = 0;
arr[0] // undefined
```


### Class的取值函数（getter）和存值函数（setter）

与ES5一样，在Class内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```javascript
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
     console.log('getter')
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

上面代码中，`prop`属性有对应的存值函数和取值函数，因此赋值和读取行为都被自定义了。

存值函数和取值函数是设置在属性的descriptor对象上的。


