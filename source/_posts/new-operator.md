---
title: new 运算符的实现
date: 2022-10-26  23:33:25
tags: [JavaScript]
category: 前端
---

> `new` 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例


`new` 关键字会进行如下的操作：

- 创建一个空的简单 `JavaScript` 对象（即{}）
- 链接该对象（设置该对象的 `constructor`）到另一个对象
- 将步骤1新创建的对象作为 `this` 的上下文
- 如果该函数没有返回对象，则返回 `this`

实现代码如下：

```javascript
function Parent(name, age) {
  this.name = name
  this.age = age
}

Parent.prototype.sayName = function () {
  return this.name
}

function myNew(fn, ...args) {
  let obj = {}
  obj.__proto__ = fn.prototype
  let res = fn.apply(obj, args)
  return typeof res === 'object' ? res : obj
}

let children = myNew(Parent, '小维', 18)

console.log(children)
```
