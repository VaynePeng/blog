---
title: 组合继承的实现
tags: [JavaScript ]
category: 前端
---

> 组合继承就是使用原型链继承原型上的属性和方法，通过盗用构造函数继承实例的属性。这样做的好处是既可以把方法定义在原型上实现重用，又可以让每个实例拥有自己的属性

实现代码如下：

```javascript
function Parent(name, age) {
  this.name = name
  this.age = age
  this.house = 150
}
Parent.prototype.sayHi = () => {
  console.log('hello world !')
}

function Children(name, age) {
  Parent.call(this, name, age) // 盗用构造函数的方法继承实例的属性，解决了原型链继承不能向父类传参的缺点和引用类型共享的问题
}
Children.prototype = Object.create(Parent.prototype) // new Parent() // 原型链继承实现了方法的重用
// 关于这个改用Object.create(),而不是使用new Parent是因为new操作会使用Parent的构造函数调用，如果Parent中存在副作用就会影响到Children的后代
Children.prototype.constructor = Children

let xiaowei = new Children('小维', 18)

console.log(xiaowei) // Children { name: '小维', age: 18, house: 150 }
xiaowei.sayHi() // hello world !
console.log(xiaowei instanceof Children) // true
console.log(xiaowei instanceof Parent) // true
```
