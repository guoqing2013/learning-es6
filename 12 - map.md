# Map

Map，类似于对象，也是键值对的集合。其中键和值可以是任意值(对象或者原始值)。

## 语法

`new Map([iterable])`

Iterable 可以是一个数组或者其他 iterable 对象，其元素或为键值对，或为两个元素的数组。


```javascript
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"


```


## Map 实例

### 属性

Map 结构的实例有以下属性。

- `size`：属性返回 Map 结构的成员总数。

### 方法


- `set(key, value)`：设置Map对象中键的值，返回该Map对象（因为返回时当前的Map对象，所以可以采用链式写法）。
- `get(key)`：返回键对应的值，如果不存在，则返回undefined。
- `has(key)`：返回一个布尔值，表示Map实例是否包含键对应的值。
- `delete(key)`：delete方法删除某个键，返回true。如果删除失败，返回false。
- `clear()`：clear方法清除所有成员，没有返回值。

- `keys()`：返回键名的遍历器。
- `values()`：返回键值的遍历器。
- `entries()`：返回所有成员的遍历器。
- `forEach(callbackFn[, thisArg])`：遍历 Map 的所有成员。 按插入顺序，为 Map对象里的每一键值对调用一次callbackFn函数。如果为forEach提供了thisArg，它将在每次回调中作为this值。


```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

`forEach`方法还可以接受第二个参数，用来绑定`this`。

```javascript
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);
```


Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

```javascript
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

