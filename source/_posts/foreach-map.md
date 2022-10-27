---
title: forEach 和 map 的实现
date: 2022-10-27  0:19:55
tags: [JavaScript]
category: 前端
---

> `forEach()` 方法对数组的每个元素执行一次给定的函数

实现代码如下：

```javascript
function _forEach(callback, thisArg) {
  let index = 0
  while (this.length > index) {
    let element = this[index]
    callback.call(thisArg, element, index, this)
    index++
  }
}

Array.prototype._forEach = _forEach
```

从上面的代码中我们不难看出，为什么 `forEach` 不能被 `return` 语句打断，我们传入的 `return` 只是终止了里面的 `callback` 函数，而外面的循环依旧在进行，对于需要提前终止循环的操作我建议都使用 `for...in...`, `for...of...` 等可以很优雅打断的遍历方式，如不是强行使用 `throw new Error()` 的方式来中断他的运行

> `map()` 方法创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成

有了上面的 `forEach` 我们只需要稍微改造一下就可以实现 `map` 了

实现代码如下：

```javascript
function _map(callback, thisArg) {
  let index = 0
  let array = []
  while (this.length > index) {
    let element = this[index]
    let item = callback.call(thisArg, element, index, this)
    array.push(item)
    index++
  }
  return array
}

Array.prototype._map = _map
```
