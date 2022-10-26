---
title: 使用 CSS 画三角形
tags: [CSS, HTML]
category: 前端
---

三角形是我们网页中比较常见的元素，比如下拉菜单，折叠面板等都有一个三角形的指示器，我们今天就使用 `CSS` 来实现这样的一个形状

不少细心（无聊）的人应该都有发现，当我们把元素的宽高设置为 0，然后给元素加上等宽的 `border` 我们就可以得到一个正方形，如果我们给 `border` 的四边不同的颜色我们就可以得到一个三角形

实现代码如下：

```javascript
<div class="box"></div>

.box {
  width: 0;
  height: 0;
  border: 50px solid black;
  border-top-color: red;
  border-left-color: blue;
  border-right-color: green;
}
```

效果如下：

![image-20221027000543562](https://raw.githubusercontent.com/VaynePeng/images/master/note/202210270005626.png)

到了这里，小伙伴们应该都已经发现了，如果此时我们把一边设置某种颜色，其他边设置透明，那么我们就可以得到一个三角形了

实现代码如下：

```javascript
<div class="box"></div>

.box {
  width: 0;
  height: 0;
  border: 50px solid transparent;
  border-top-color: red;
}
```

效果如下：

![image-20221027000959504](https://raw.githubusercontent.com/VaynePeng/images/master/note/202210270009520.png)

好了，到这里我们的三角形就已经画好了，其实 `border` 还会根据作用元素的形状改变自身的形状，当元素为正方形时，`border` 为一个梯形，当元素为圆形时，`border` 为一个扇形，到这里不如动手去试试更多可能性吧
