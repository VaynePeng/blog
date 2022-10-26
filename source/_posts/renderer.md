---
title: renderer 函数的简单实现
tags: [JavaScript, Vue]
category: 前端
---

前段时间看了一点点 Vue 相关的资料，看到了 renderer 函数的实现，就跟着写了一个最基础的实现

实现代码如下：

```javascript
let VNode = {
  tag: 'div',
  props: {
    className: 'box',
    onclick: () => alert('create VNode success !')
  },
  children: [
    {
      tag: 'span',
      children: 'click me !'
    }
  ]
}

function render(VNode, tilePort = document.body) {
  // 创建当前节点
  let element = document.createElement(VNode.tag)
  for (const key in VNode.props) {
    if (/^on/.test(key)) {
      // 绑定事件
      element.addEventListener(key.slice(2).toLowerCase(), VNode.props[key])
    } else {
      // 绑定元素属性
      element[key] = VNode.props[key]
    }
  }
  // 渲染children
  if (typeof VNode.children === 'string') {
    // 渲染一个字符串
    element.appendChild(document.createTextNode(VNode.children))
  } else if (Array.isArray(VNode.children)) {
    VNode.children.forEach((el) => {
      render(el, element)
    })
  }
  tilePort.appendChild(element)
}

render(VNode)
```
