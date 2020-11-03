---
title: Less
toc: true
categories: [前端, CSS, Less]
tags: [CSS]
date: 2018-04-01 17:58:01
cover: /images/covers/5.webp
---

<a name="Bk1ac"></a>
## 1、variable

<a name="OANN0"></a>
### 1.1、通用

```css
@link-color: #08aaef;
@hover-color: darken(@link-color, 10%);

a,
a.link {
    color: @link-color;
}

a:hover {
    color: @hover-color;
}
```

输出为：

```css
a,
a.link {
  color: #08aaef;
}
a:hover {
  color: #0687be;
}
```

<a name="kPKDc"></a>
### 1.2、插值

上面的例子着重于使用变量来控制 CSS 规则中的值，但是它们也可以在其他地方使用，例如选择器名称、属性名称、url 和 @import 语句。

插值的使用形式为  `@{variable-name}` 拼接其他字符串或者单独使用。

<a name="hK024"></a>
#### 1.2.1、选择器

```css
@btn-prefix: ivu;

.@{btn-prefix}-primary {
    border-radius: 5px;
    border: 1px solid #08aaef;
    color: #08aaef;
}
```

输出为：

```css
.ivu-primary {
  border-radius: 5px;
  border: 1px solid #08aaef;
  color: #08aaef;
}
```

<a name="CkwMZ"></a>
#### 1.2.2、属性名

```css
@property-color: color;

.widget {
    @{property-color}: #08aaef;
    background-@{property-color}: #e1e1e1;
}
```

输出为：

```css
.widget {
  color: #08aaef;
  background-color: #e1e1e1;
}
```

<a name="zinlA"></a>
#### 1.2.3、URLs

```css
@images: '../../assets/images';

.banner {
    background: url('@{images}/home/bg.png') no-repeat;
    background-size: 100% 100%;
}
```

输出为：

```css
.banner {
  background: url('../../assets/images/home/bg.png') no-repeat;
  background-size: 100% 100%;
}
```

<a name="u98lt"></a>
#### 1.2.4、import

```css
@styles: '../../assets/styles';

@import '@{styles}/variable.less';
@import '@{styles}/mixin.less';
```

<a name="TGBgc"></a>
### 1.3、变量中的变量

在Less中，可以使用另一个变量定义一个变量的名称。

```css
@primary:  green;
@secondary: blue;

.section {
  @color: primary;

  .element {
    color: @@color;
  }
}
```

输出为：

```css
.section .element {
  color: green;
}
```

<a name="ZvlHn"></a>
### 1.4、变量提升

- Less 中的变量提升与 JavaScript 语言中的变量提升相同
- Less 中的作用域原则与 JavaScript 语言中的作用域原则相同

<a name="yNef3"></a>
## 2、&

<a name="BbUyZ"></a>
### 2.1、通用

`&` 运算符表示嵌套规则中对父选择器的引用。

```css
a {
    color: blue;

    &:hover {
        color: lighten(blue, 10%);
    }
}
```

输出为：

```css
a {
  color: blue;
}
a:hover {
  color: #3333ff;
}
```

<a name="x79qy"></a>
### 2.2、插值

```css
.button {
    &-ok {
        color: green;
    }

    &-warning {
        color: yellow;
    }

    &-error {
        color: red;
    }
}
```

输出为：

```css
.button-ok {
  color: green;
}
.button-warning {
  color: yellow;
}
.button-error {
  color: red;
}
```

<a name="PSRNk"></a>
### 2.3、复合

```css
.button {
    & & {
        color: yellow;
    }

    &>& {
        color: black;
    }

    &+& {
        color: white;
    }

    && {
        color: red;
    }
}
```

输出为：

```css
.button .button {
  color: yellow;
}
.button > .button {
  color: black;
}
.button + .button {
  color: white;
}
.button.button {
  color: red;
}
```

<a name="1RlUm"></a>
### 2.4、改变顺序

```css
div {
    a & {
        color: red;
    }
}
```

输出为：

```css
a div {
  color: red;
}
```

<a name="cnkOf"></a>
### 2.5、组合

```css
p, div, a {
    & + & {
        color: red;
    }
}
```

输出为：

```css
p + p,
p + div,
p + a,
div + p,
div + div,
div + a,
a + p,
a + div,
a + a {
  color: red;
}
```

<a name="Z7n8R"></a>
### 2.6、注意

`&` 表示所有嵌套选择器，而不是最近的父级。如下：

```css
div {
    p {
        a {
            &:hover {
                color: white;
            }
        }
    }
}
```

输出为：

```css
div p a:hover {
  color: white;
}
```

而不是：

```css
a:hover {
  color: white;
}
```

<a name="BvqwQ"></a>
## 3、extend

`extend` 是 less 中的一个伪类语法，负责将一个或一些选择器中的声明继承到指定选择器中。

<a name="TAOHT"></a>
### 3.1、语法

```css
.b { color:red; }
.a:extend(.b) { background: white; }
```

意为 a 选择器将继承 b 选择器的规则集，输出为：

```css
.b, .a { color:red; }
.a { background: white; }
```

该语法的形式有如下几种：

- 跟在选择器之后：`.a:extend(.b)`
- 带空格： `.a :extend(.b)`
- 多个串联： `.a:extend(.f):extend(.g)`
- 只能跟在最后：因此 `pre:hover:extend(div pre):nth-child(odd)` 是不允许的

> 1. `pre:hover:extend(.a):extend(.b)` 等同于 `pre:hover:extend(.a,.b)`
> 1. 选择器嵌套情形遵从上面原则


<a name="K2dil"></a>
### 3.2、多重继承

```css
.a:extend(.f){}
.a:extend(.g){}

// 等同于
.a:extend(.f,.g){}
```

统一输出为：

```css
.f,.a {}
.g,.a {}
.a {}
```

<a name="0idxM"></a>
### 3.3、精准匹配

默认情况下，less 在为 extend 查找继承选择器时，遵从精准匹配策略。即选择器形式必须完全统一才能匹配，如下列举的情况：

- `.text:extend(.class)`  与 `.a.class` 、`.class.a` 、`.class > .a` 、`.a > .class`、 `*.class` 不匹配
- `.text:extend(a:hover:visited)` 与`a:visited:hover` 不匹配
- `.text:extend(:nth-child( 1n + 3 ))` 与 `:nth-child(n+3)` 不匹配

但属性选择器的单双引号情况例外：

- `.text:extend([title=identifier])` 与 `[title='identifier']` 和 `[title="identifier"]` 匹配

<a name="Ifh4p"></a>
### 3.4、all

```css
.a.b.test,
.test.c {
  color: orange;
}
.test {
  &:hover {
    color: green;
  }
}

.replacement:extend(.test all) {}
```

输出为：

```css
.a.b.test,
.test.c,
.a.b.replacement,
.replacement.c {
  color: orange;
}
.test:hover,
.replacement:hover {
  color: green;
}
```

<a name="p4io3"></a>
### 3.5、选择器插值

略

<a name="LgGzh"></a>
### 3.6、media 作用域

作用域规则可类比 JavaScript 语言中的变量作用域，以 media 花括号为作用域范畴。

<a name="YtSLX"></a>
### 3.7、经典场景

<a name="N7fR4"></a>
#### 3.7.1、减少基类

```css
// CSS
.animal {
    color: white;
    background-color: red;
}

.bear {
    color: black;
}

// HTML
<a classs="animal bear"></a>
```

可改为：

```css
// CSS
.animal {
    color: white;
    background-color: red;
}

.bear {
  	&:extend(.animal);
    color: black;
}

// HTML
<a classs="bear"></a>
```

<a name="sETj5"></a>
#### 3.7.2、减少重复

```css
.my-inline-block() {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  .my-inline-block;
}
.thing2 {
  .my-inline-block;
}

// 输出为
.thing1 {
  display: inline-block;
  font-size: 0;
}
.thing2 {
  display: inline-block;
  font-size: 0;
}
```

可改为：

```css
.my-inline-block {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  &:extend(.my-inline-block);
}
.thing2 {
  &:extend(.my-inline-block);
}

// 输出为
.my-inline-block,
.thing1,
.thing2 {
  display: inline-block;
  font-size: 0;
}
```

<a name="hvXub"></a>
## 4、merge

合并：将多个属性的值合并到单个属性下的逗号或空格分隔的列表中，合并对于诸如背景和变换之类的属性很有用。

```css
// 不合并
.mixin() {
    box-shadow: inset 0 1px 10px #e55;
}

.class {
    .mixin();
    box-shadow: 0 0 20px black;
}

// 输出为
.class {
    box-shadow: inset 0 1px 10px #e55;
    box-shadow: 0 0 20px black;
}

// 合并
.mixin() {
    box-shadow+: inset 0 1px 10px #e55;
}

.class {
    .mixin();
    box-shadow+: 0 0 20px black;
}

// 输出为
.class {
  box-shadow: inset 0 1px 10px #e55, 0 0 20px black;
}
```

**语法：**

- 逗号分隔： `+` 
- 空格分隔： `+-` 

<a name="wt1b5"></a>
## 5、mixins

<a name="tk3k1"></a>
### 5.1、语法

```css
.class-name, #id-name {
    color: red;
}

.mixin-class {
    .class-name();
}

.mixin-id {
    #id-name();
}
```

输出为：

```css
.class-name,
#id-name {
  color: red;
}
.mixin-class {
  color: red;
}
.mixin-id {
  color: red;
}
```

<a name="hWGnK"></a>
### 5.2、不输出

```css
.mixin-a {
    color: red;
}

.mixin-b() {
    color: green();
}

.class-a {
    .mixin-a();
    .mixin-b();
}
```

输出为：

```css
.mixin-a {
  color: red;
}
.class-a {
  color: red;
  color: green;
}
```

<a name="bvIJv"></a>
### 5.3、Selectors in Mixins

Mixins 不仅可以包含属性，还可以包含其他的选择器。

```css
.mixin-hover() {
    color: red;

    &:hover {
        background-color: white;
    }
}

button {
    .mixin-hover();
}
```

输出为：

```css
button {
  color: red;
}
button:hover {
  background-color: white;
}
```

<a name="9Gagf"></a>
### 5.4、Namespace

- 减少与其他库混合使用时的冲突
- 一种组织 mixins 的方法

```css
#outer() {
  .inner-a {
    color: red;
  }
  .inner-b {
    color: green;
  }
}

.c {
  #outer > .inner-a();
}
```

<a name="7TrK6"></a>
### 5.5、守卫

```css
#namespace when (@mode = huge) {
  .mixin() { /* */ }
}

#namespace {
  .mixin() when (@mode = huge) { /* */ }
}
```

<a name="xN2O8"></a>
### 5.6、important

```css
.foo(@bg: #e55, @color: #4e5) {
    background-color: @bg;
    color: @color;
}

.unimportant {
    .foo();
}

.important {
    .foo() !important;
}
```

输出为：

```css
.unimportant {
  background-color: #e55;
  color: #4e5;
}
.important {
  background-color: #e55 !important;
  color: #4e5 !important;
}
```

<a name="LXlSD"></a>
### 5.7、传参数

略，待补充。

<a name="qf7Q5"></a>
## 6、import

- `@import` 语句可以在文档的任何位置（区别于 css 中的只能在头部）
- 根据文件扩展名的不同， `import` 语句的处理方式可能会有所不同
   - @import "foo";      // foo.less is imported
   - @import "foo.less"; // foo.less is imported
   - @import "foo.php";  // foo.php imported as a Less file
   - @import "foo.css";  // statement left in place, as-is

<a name="MV4p0"></a>
### 6.1、选项

Syntax: `@import (keyword) "filename";`

- `reference`: use a Less file but do not output it
- `inline`: include the source file in the output but do not process it
- `less`: treat the file as a Less file, no matter what the file extension
- `css`: treat the file as a CSS file, no matter what the file extension
- `once`: only include the file once (this is default behavior)
- `multiple`: include the file multiple times
- `optional`: continue compiling when file is not found

<a name="Mua4F"></a>
## 7、Maps

略，待补充。
