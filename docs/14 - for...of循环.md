# for...of循环

一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有iterator接口，就可以用`for...of`循环遍历它的成员。也就是说，`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法。

`for...of`循环语句通过方法调用来遍历各种集合，包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、 Generator 对象，以及字符串。

