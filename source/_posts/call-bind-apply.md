---
title: call, bind, apply 的实现
tags: [JavaScript]
category: 前端
---

公用示例代码:

```javascript
let person = {
  name: '小维',
  job: '前端',
  say(params) {
    console.log(`${this.name}是一个${this.job}${params}`)
  }
}

let person2 = {
  name: '小红',
  job: '项目经理'
}
```

> `call()` 方法使用一个指定的 `this` 值和单独给出的一个或多个参数来调用一个函数

实现代码如下：

```javascript
Function.prototype.MyCall = function (context, ...args) {
  context = context || window // 判断this指向，有参数指向参数，无参数指向window
  let fn = Symbol() // 定义一个方法名称，为了防止覆盖原来的方法，使用Symbol
  context[fn] = this // 这里的this指向调用的方法，然后给传递过来的对象新增这个方法
  let res = context[fn](...args) // 调用方法
  delete context[fn] // 调用完之后删除新建的方法
  return res
}

person.say.MyCall(person2, '@') // 小红是一个项目经理@
```

> `apply()` 方法调用一个具有给定 `this` 值的函数，以及以一个数组（或一个类数组对象）的形式提供的参数

从概念上看来，`call` 和 `apply` 非常相似，仅接受参数的形式不同，将接受多个参数改成数组即可，所以写起来也非常容易

实现代码如下：

```javascript
Function.prototype.MyApply = function (context, args) {
  context = context || window // 判断this指向，有参数指向参数，无参数指向window
  let fn = Symbol() // 定义一个方法名称，为了防止覆盖原来的方法，使用Symbol
  context[fn] = this // 这里的this指向调用的方法，然后给传递过来的对象新增这个方法
  let res = context[fn](...args) // 调用方法
  delete context[fn] // 调用完之后删除新建的方法
  return res
}

person.say.MyApply(person2, ['$']) // 小红是一个项目经理$
```

> `bind()` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用

不同的是，`bind` 返回一个可调用的函数，我们可以之前返回之前写好的 `apply` 或者 `call` 即可

实现代码如下：

```javascript
Function.prototype.MyBind = function(context) {
  context = context || window // 判断this指向，有参数指向参数，无参数指向window
  let self = this // 返回一个绑定this的函数，我们需要在此保存this
  // 返回一个函数
  return function(...args) {
    self.apply(context, args)
  }
}

let mb = person.say.MyBind(person2)
mb('*') // 小红是一个项目经理*
```
