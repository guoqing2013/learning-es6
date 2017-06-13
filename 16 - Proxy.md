http://www.zcfy.cc/article/6-compelling-use-cases-for-es6-proxies-888.html

http://es6.ruanyifeng.com/#docs/proxy

https://raw.githubusercontent.com/ruanyf/es6tutorial/gh-pages/docs/proxy.md

# Proxy

Proxy 用于拦截对象上常用的方法和属性。最常见的有get，set，apply和construct。

## 语法

`let p = new Proxy(target, handler);`

参数：

target：　所要拦截的目标对象 , 所要代理的目标对象(可以是任何类型的对象，包括本机数组，函数，甚至另一个代理)

handler：也是一个对象，用来定制拦截行为



```javascript
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

proxy.time // 35
proxy.name // 35
proxy.title // 35
```


同一个拦截器函数，可以设置拦截多个操作。

```javascript
var handler = {
  get: function(target, name) {
    if (name === 'prototype') {
      return Object.prototype;
    }
    return 'Hello, ' + name;
  },

  apply: function(target, thisBinding, args) {
    return args[0];
  },

  construct: function(target, args) {
    return {value: args[1]};
  }
};

var fproxy = new Proxy(function(x, y) {
  return x + y;
}, handler);

fproxy(1, 2) // 1
new fproxy(1,2) // {value: 2}
fproxy.prototype === Object.prototype // true
fproxy.foo // "Hello, foo"
```


## 使用案例

### 1. 剥离验证逻辑

