# js基础-核心知识

## 1. 数据类型 一共8种，速记法：8大金刚、8仙过海

   1. 值类型(基本类型)：字符串（String）、数字(Number)、布尔(Boolean)、空（Null）、未定义（Undefined）、Symbol (ES6)、BigInt(ES2010) 只用来表示整数，没有位数的限制，可以表示任意大的整数。
   值类型-原始类型占用空间固定, 保存在栈中。

   2. 引用数据类型：对象(Object)、数组(Array)、函数(Function)。占用空间不固定, 保存在堆中

 > 注：Symbol 是 ES6 引入了一种新的原始数据类型，表示独一无二的值。不支持 new Symbol(); 
* [ ] 文档：
  + [JavaScript 数据类型和数据结构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

 [延伸-js类型判断]('www.baidu.com')

## 2. 闭包 

### 有权访问另一个函数作用域中的变量的函数

```js
let y = 1;

function(y) {
    //包含Y的作用域
    return function(x) {
        //y是自由变量  返回值依赖于自由变量 
        return x + y;
    }

}

function fun1() {
    let a = 1;
    return function() {
        console.log(a);
    }
}
fun1();
let result = fun1();
reslut();

//IIFE 
var c = 2;
(function bibao() {
    console.log(c); //22
})()
```

应用场景
1. 返回一个函数 内部函数没执行完 外部函数的内存空间就不回销毁
2. 在定时器 、时间监听、Ajax请求、webworkers或者任何异步中操作中只要使用了回调函数 就是闭包
3. 作为函数参数传递
4. IIFE 立即执行函数  创建了闭包 保存了全局作用域 window 和当前函数的作用域 因此可以输出全局的变量

```js
for (var i = 1; i <= 5; i++) {
    setTimeout(function(j) {
        console.log(j);
    }, 0, i)
}
```

## 3. this 

 ### [深入理解this]('https://segmentfault.com/a/1190000011194676')
 1. this是JavaScript的关键字之一。它是对象自动生成的一个内部对象，只能在 对象 内部使用。随着函数使用场合的不同，this的值会发生变化。
2. this指向什么，完全取决于 什么地方以什么方式调用，而不是创建时。（比较多人误解的地方）
3. 构造函数中的this  指向new创建的对象

## 4. 原型

```js
//构造函数的属性
function Animal() {

}
Animal.prototype.sayName = function() {
    console.log(name)
}

function Dog(name) {
    this.name = name;
}
Dog.prototype = New Animal();
var dog = New Dog();
```

原型是一个对象，所有对象都会从原型对象上继承属性和方法
原型链 对象的原型还有自己的原型 寻找特性属性的时候 先去对象里面找 找不到就对象的原型上找   
原型链 终点 Object.prototype  只要在Object对象的原型上添加属性和方法，所有的对象都能使用

## 5. 类

```js
//ES5 类通过构造函数完成
function Dog(name) {
    this.name = name;
}
var dog = new Dog('旺财');

//es6 类

class Dog {
    constructor(name) {
        this.name = name;
    }
}

//继承
class animal extand Dog {
    constructor(name, age) {
        super(name); //继承父类的属性
        this.age = age;
    }
}
```

## 6. 事件

### 事件绑定

1. addEventListener
addEventListener(eventType, fun, boolean); //默认false : 冒泡阶段触发  true : 捕获阶段触发

```js
el.addEventListener("click", function() {

}, false)
//事件捕获与冒泡  默认情况下 事件会在冒泡阶段执行
```

2. element.onEventType=fun

```js
el.onclick = function() {}
```

区别:
1.addEventListener在同一元素上的同一事件类型添加多个事件，不会被覆盖，而onEventType会被覆盖
2.addEventListener可以设置元素在捕获阶段触发事件，而onEventType不能

### 阻止冒泡 

e.stopPropagation(); 

### 事件默认行为

去掉默认行为 e.perventDefault return false 

### 事件委托

通过e.target将子元素的事件委托给父级处理

## 7. 事件循环 

[一次弄懂Event Loop](https://juejin.cn/post/6844903764202094606#heading-0)

 | macro-task(宏任务)  | micro-task(微任务) |
 | -----------------  | ------------------ |
 | setTimeout | promise.then|
 | setInterval I/O | process.nextTick（Node环境）|
 | 事件队列 |
 | script（整体代码块）| process.nextTick（Node环境）|

     

代码开始执行，创建一个全局调用栈，script作为宏任务执行
执行过程过同步任务立即执行，异步任务根据异步任务类型分别注册到微任务队列和宏任务队列
同步任务执行完毕，查看微任务队列
若存在微任务，将微任务队列全部执行(包括执行微任务过程中产生的新微任务)
若无微任务，查看宏任务队列，执行第一个宏任务，宏任务执行完毕，查看微任务队列，重复上述操作，直至宏任务队列为空

![时间循环](./image/eventloop2.png '事件循环示意图')

1. javascript是一门单线程语言，不管是什么新框架新语法糖实现的所谓异步，其实都是用同步的方法去模拟的，牢牢把握住单线程这点非常重要。
2. 事件循环是js实现异步的一种方法，也是js的执行机制。
3. javascript是一门单线程语言 Event Loop是javascript的执行机制

## 8. 位运算

## 9. call apply bind

### call apply bind都是函数的方法  改变函数内部this指向

1. call 会立即执行 参数依次传递
2. apply 会立即执行 参数数组方式传递
3. bind 不会立即执行 将重新绑定this的函数作为返回值  参数依次传递

```js
 let obj = {
     name: 'nidaye'
 }
 let obj1 = {
     name: 'daye',
     sayName(food, food2) {
         console.log(`我是${this.name},我****想吃${food}和${food2}`);
     }
 }
 obj1.sayName('猪脚饭', '螺蛳粉');
 obj1.sayName.call(obj, '鱼', '老鼠') //改变指向  我是nidaye,我****想吃鱼和老鼠 
 // apply 参数传递方式不同 数组方式[]    bind
 obj1.sayName.apply(obj, ['鱼', '大老鼠'])
 let newObj1 = obj1.sayName.bind(obj, '鱼', '狗粮'); //不会执行
 // newObj1();//这里bind改变this的函数才会执行
 obj1.sayName.bind(obj, '鱼', '狗粮啊')();
```

call 参数 一个个传  apply参数是数组的形式

## 10. promise

[手写promise](https://juejin.cn/post/6945319439772434469#heading-0)
1. promise 初始状态 pending  
2. 状态可以由 pengding->fulfilled  pending->rejected  修改了就不能变
3. 成功resolve 失败reject

```js
let promise = new Promise((resolve, reject) => {

})

promise.then(res => {},
    catch (err => {}))
```

### 11. 迭代器与生成器

### 什么是 JavaScript 生成器？

生成器是一种可以用来控制迭代器（iterator）的函数，它可以随时暂停，并可以在任意时候恢复。
* [ ] 推荐文章
  + [ ] [[译] 什么是 JavaScript 生成器？如何使用生成器？](https://juejin.cn/post/6844903616357072910)

## 12. 模块化

1. AMD  requireJs  依赖前置
2. CMD  Seajs  依赖就近
3. CommonJS 

AMD CommonJs 都是运行时加载 
module.exports和exports

ES6 module

```js
// 1. 导出
const api = '666';
export {
    api
}
// 导入   a.js
import {
    api
} from './config.js';
// or
// 配合`import`使用的`as`关键字用来为导入的接口重命名。
import {
    api as myApi
} from './config.js';
// （整体导入）
import * as config from './config.js';

// 2. 默认导入  导出  foo.js
export const conut = 0;
export default function myFoo() {}
//默认导出的导入  import name form 'module'
// 默认导入的接口此处刻意命名为cusFoo，旨在说明该命名可完全自定义。
import cusFoo, {
    count
} from './foo.js';
// 等同于：
import {
    default as cusFoo,
    count
} from './foo.js';
```

[模块化](https://segmentfault.com/a/1190000023711059)


