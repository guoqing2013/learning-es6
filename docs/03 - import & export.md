# Module

到目前为止,javascript (ES5及以前) 还不能支持原生的模块化，大多数的解决方案都是通过引用外部的库来实现模块化。比如 遵循CMD规范的 Seajs 和AMD的 RequireJS 。在ES6中，模块将作为重要的组成部分被添加进来。模块的功能主要由 export 和 import 组成.每一个模块都有自己单独的作用域，模块之间的相互调用关系是通过 export 来规定模块对外暴露的接口，通过import来引用其它模块提供的接口。同时还为模块创造了命名空间，防止函数的命名冲突。

## export,import 命令

```javascript
//test.js
export var name = 'Rainbow'
```


ES6将一个文件视为一个模块，上面的模块通过 **export** 向外输出了一个变量。一个模块也可以同时往外面输出多个变量。

```javascript
//test.js
var name = 'Rainbow';
var age = '24';
export {name, age};
```


定义好模块的输出以后就可以在另外一个模块通过 **import** 引用。

```javascript
//index.js
import {name, age} from './test.js'
```


## 整体输入，module指令

```javascript
//test.js

export function getName() {
	return name;
}
export function getAge(){
	return age;
} 
```

通过 **import * as** 就完成了模块整体的导入。

```javascript
import * as test form './test.js';
```

通过指令 **module** 也可以达到整体的输入。

```javascript
module test from 'test.js';
test.getName();
```


## export default

不用关系模块输出了什么，通过 **export default** 指令就能加载到默认模块，不需要通过花括号来指定输出的模块,一个模块只能使用 export default 一次

```javascript
// default 导出
export default function getAge() {} 

// 或者写成
function getAge() {}
export default getAge;

// 导入的时候不需要花括号
import test from './test.js';
```


一条import 语句可以同时导入默认方法和其它变量.
```javascript
import defaultMethod, { otherMethod } from 'xxx.js';
```