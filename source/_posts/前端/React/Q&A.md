---
title: Q&A
toc: true
categories: [前端, React]
---


<br />**待解答：**

1. vue数据双向绑定原理(对象、defineProperty、set/get、数组、.sync)
1. vue computed原理、
1. vue编译器结构图、
1. 生命周期、
1. vue组件通信、
1. mmvm模式、
1. mvc模式理解、
1. vue dom diff、
1. vuex、
1. vue-router
1. sync 修饰符的用处
1. 怎么理解单项数据流
1. 双向绑定 v-model 是什么？怎么自定义？和 react 受控组件的对比
1. v-slot 如何使用？作用域是什么？
1. 依赖注入怎么用？

<br />
<a name="jBJGj"></a>
#### 1. v-for 和 v-if 同时存在会怎样？

<br />for 和 if 作用在同一个模板节点上，会有两种意图：<br />

- for 包裹 if
```javascript
for (let i = 0; i < arr.length; i++) {
  if(i < 3) {
    // some
  }
}
```


- if 包裹 for
```javascript
if(expression) {
  for (let i = 0; i < arr.length(); i++) {
    // some
  }
}
```

<br />一般不要这样用，但如果这样使用了，vue 会采用第一种逻辑处理。<br />
<br />

<a name="BabI3"></a>
#### 2. 如何理解单项数据流和操作


<a name="cn5ZJ"></a>
#### 3. 计算属性和方法的区别
