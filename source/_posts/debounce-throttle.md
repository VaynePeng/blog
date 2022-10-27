---
title: 防抖和节流
date: 2022-10-26  20:04:26
tags: [JavaScript]
category: 前端
---

> 防抖：触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间

防抖可以用 `setTimeout` 来完成，然后每次触发事件时都取消之前的延时调用方法就可以了

实现代码如下：

```javascript
sayHello() {
  console.log('我被调用啦！！！')
}
// 防抖
function debounce(fn) {
  let timeout = null
  return function () {
    if (timeout === null) {
      fn.apply(this, arguments) // 使用apply让fn指向调用debounce的对象，不写将导致fn的this将指向window
    }
    clearTimeout(timeout)
    timeout = setTimeout(() => {
      timeout = null
    }, 1000)
  }
}
let btn = document.getElementById('debounce')
btn.addEventListener('click',debounce(sayHello),false)
```

> 节流：高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率

节流我们还是可以用 `setTimeout` 的方式来完成，定义一个延时标志，每次触发事件时都判断当前是否有等待执行的延时函数，有就继续等待，没有就再次调用

实现代码如下：

```javascript
sayHello() {
  console.log('我被调用啦！！！')
}
function throttle(fn) {
  let run = false
  return function () {
    if (run) {
      return
    }
    fn.apply(this, arguments)
    run = true
    setTimeout(() => {
      run = false
    }, 1000)
  }
}
let btn = document.getElementById('throttle')
btn.addEventListener('click',throttle(sayHello),false)
```
