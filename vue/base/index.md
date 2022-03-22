vue知识点大全
01. MVVM
02. 双向绑定原理
03. 常用指令 v-model v-if v-for v-show v-html v-text
04. 组件通信方式 8种 父子 兄弟  
05. vuex 
06. mixin
07. computed
08. watch
09. router
10. 生命周期 挂载前后  加载前后   更新前后 销毁前后
11. 插槽
12. template 
   模板编译原理  使用正则等方式解析成抽象语法树（AST）
13. keep-alive


## 其他
```js
v - model 修饰符
lazy:
    默认情况下， v - model在进行双向绑定时， 绑定的是input事件， 那么会在每次内容输入后就将最新的值和绑定的属性进行同步；
如果我们在v - model后跟上lazy修饰符， 那么会将绑定的事件切换为 change 事件， 只有在提交时（ 比如回车） 才会触发化

number: 对输入进行类型转换
trim: 去除前后空白字符
```
## Vue2 vue3
 [Object.defineProperty 与 Proxy 对比](https://juejin.cn/post/7069397770766909476#heading-1)



```
