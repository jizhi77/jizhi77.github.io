---
title: Flex
toc: true
categories: [前端, CSS]
tags: [CSS]
date: 2017-03-05 08:09:12
cover: /images/covers/3.webp
---

Flex 是 flexible box （可伸缩盒子）的简称。

传统的布局解决方案：

- 基于 table（CSS3.0 未实现之前的方案，已废弃）
- 基于盒模型，依赖于 `display` 、 `position` 、 `float` 属性

传统布局多少有失灵活性和不方便性，flex 则进行了相应的提升。

## 1、基本概念
采用 Flex 布局的元素，称为 Flex 容器，他的所有子元素自动成为容器成员，称为 Flex 项目（flex item）。


![image.png](https://cdn.nlark.com/yuque/0/2020/png/85733/1581251212213-ef36c013-0423-4f32-914b-5f1780494a73.png#align=left&display=inline&height=333&name=image.png&originHeight=333&originWidth=563&size=30491&status=done&style=none&width=563)


容器默认存在两根轴：主轴（main axis）和交叉轴（cross axis）

- 主轴的开始位置（与边框的交叉点）叫做 main start，结束位置叫做 main end
- 交叉轴的开始位置叫做 cross start，结束位置叫做 cross end
- 项目默认沿主轴排列，单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size

## 2. 容器属性

Flex 容器有以下 6 个属性：

1. flex-direction
1. flex-wrap
1. flex-flow
1. justify-content
1. align-items
1. align-content

### 2.1、flex-direction

flex-direction 属性决定主轴的方向（即项目的排列方向）：

```css
.box {
    flex-direction: row | row-reverse | column | column-reverse;
}
```




### 2.2、flex-wrap

flex-wrap 决定 flex item 超出一排长度时是否换行。

- 默认为不换行
- flex item 对于宽度的分割按照 `width` 、 `flex` 属性的定义，如果都为 width 而且超出了，则会被压缩，否则按照 `flex` 属性的定义分割

```css
.box {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```


### 2.3、flex-flow

`flex-flow`  是 `flex-direction` 和 `flex-wrap` 的缩写。

```css
.box {
    flex-flow: <flex-direction> | <flex-wrap> ;
}
```

### 2.4、justify-content

`justify-content` 属性定义了 `flex-item` 在主轴上的对齐方式。

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}
```



### 2.5、align-items

`align-items` 属性定义 `flex item` 在交叉轴上如何排列。

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/85733/1581255471998-060a0a4b-9426-4159-a609-53f40cbe02e9.png#align=left&display=inline&height=786&name=image.png&originHeight=786&originWidth=617&size=28890&status=done&style=none&width=617)
### 2.6、align-content

`align-content` 属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch;
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/85733/1581255592495-eb41d103-e00a-4b2d-8788-086172972e40.png#align=left&display=inline&height=786&name=image.png&originHeight=786&originWidth=620&size=28336&status=done&style=none&width=620)
## 3、项目属性

Flex 容器有以下 6 个属性：

1. order
1. flex-grow
1. flex-shrink
1. flex-basis
1. flex
1. align-self
### 
### 3.1、order

`order` 属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

### 3.2、flex-grow

`flex-grow` 属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

比如：如果所有项目的 `flex-grow` 属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的 `flex-grow` 属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

### 3.3、flex-shrink

`flex-shrink` 属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

比如：如果所有项目的 `flex-shrink` 属性都为1，当空间不足时，都将等比例缩小。如果一个项目的 `flex-shrink` 属性为0，其他项目都为1，则空间不足时，前者不缩小。

### 3.4、flex-basis

`flex-basis` 属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为 `auto` ，即项目的本来大小。

它可以设为跟 `width` 或 `height` 属性一样的值（比如350px），则项目将占据固定空间。

### 3.5、flex

`flex` 属性是 `flex-grow` , ` flex-shrink`  和 `flex-basis` 的简写，默认值为 `0 1 auto` 。后两个属性可选。

### 3.6、align-self

`align-self` 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 `align-items` 属性。默认值为 `auto` ，表示继承父元素的 `align-items` 属性，如果没有父元素，则等同于 `stretch` 。



> 参考文档：
> 1. [https://xluos.github.io/demo/flexbox/](https://xluos.github.io/demo/flexbox/)
> 1. [http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)


