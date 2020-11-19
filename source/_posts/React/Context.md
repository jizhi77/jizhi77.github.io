---
title: 说说 React 系列之：Context
date: 2020-11-18 23:37:26
tags: [React]
categories: [React]
cover: /images/covers/33.webp
toc: true
thumbnail: /images/covers/33.webp
---

<article class="message is-info">
  <div class="message-header">
    <p>引言</p>
  </div>
  <div class="message-body">
    <ol>
        <li>Context 设计的目的是为了共享那些对于同一组件树而言是"全局"的数据</li>
        <li>Context 主要应用场景在于很多不同层级的组件需要访问同样的数据</li>
    </ol>
  </div>
</article>

上面 [组件通信方式](/2020/11/16/React/组件通信方式/) 一文讲过，React 中组件之间传递数据的形式无非`props`、`Context`、`redux` 等几种，每种方式都有其应用的场景。如果：
 1. 既不想使用 redux，因为其太复杂（一堆的 action、reducer）
 2. 又被跨级多组件共享状态时传递`props`搞的晕头转向的情况

那么此时就可以考虑使用`Context` 这个特性了。

## 使用姿势

### 姿势一

> 姿势一：只创建`Context`对象，组件引用并读取`Context`的值

1、`Context.js`提供 `Context` 容器。

```jsx Context.js
import React from "react";
export const ThemeContext = React.createContext('red')
```

2、其他组件引用容器并声明`contextType`属性后读取`Context`容器的默认值。

```jsx Child.js
import React, { Component } from "react";
import SecondChild from './SecondChild'

import { ThemeContext } from './res/Context'

class Child extends Component {
    static contextType = ThemeContext

    render() {
        return (
            <div className="childWrap">
                <div>
                    Child 组件的主题色：<span style={{ color: this.context }}>{this.context}</span>
                </div>
                <SecondChild />
            </div>
        )
    }
}

export default Child
```

```jsx SecondChild.js
import React, { Component } from "react";
import { ThemeContext } from './res/Context'

class SecondChild extends Component {

    static contextType = ThemeContext

    render() {
        return (
            <div className="secondChildWrap">
                SecondChild 组件的主题色<span style={{ color: this.context }}>{this.context}</span>
            </div>
        )
    }
}

export default SecondChild
```

### 姿势二

> 创建`Context`对象，顶级组件提供`Provider`并赋予新值。


1、`Context.js`提供 `Context` 容器。

```jsx Context.js
import React from "react";
export const ThemeContext = React.createContext('red')
```

2、顶级组件提供 `Provider`

```jsx index.js
import React from 'react'
import Child from './Child'

import { ThemeContext } from './res/Context'

class ContextDemo extends React.Component {

    render() {
        return (
            <ThemeContext.Provider value={'yellow'}>
                <div className="homeWrap">
                    <Child />
                </div>
            </ThemeContext.Provider>
        )
    }
}

export default ContextDemo
```

3、其他组件引用容器并声明`contextType`属性后读取`Context`容器的默认值。

```jsx Child.js
import React, { Component } from "react";
import SecondChild from './SecondChild'

import { ThemeContext } from './res/Context'

class Child extends Component {
    static contextType = ThemeContext

    render() {
        return (
            <div className="childWrap">
                <div>
                    Child 组件的主题色：<span style={{ color: this.context }}>{this.context}</span>
                </div>
                <SecondChild />
            </div>
        )
    }
}

export default Child
```

```jsx SecondChild.js
import React, { Component } from "react";
import { ThemeContext } from './res/Context'

class SecondChild extends Component {

    static contextType = ThemeContext

    render() {
        return (
            <div className="secondChildWrap">
                SecondChild 组件的主题色<span style={{ color: this.context }}>{this.context}</span>
            </div>
        )
    }
}

export default SecondChild
```

### 姿势三

> 创建`Context`对象，顶级组件提供`Provider`并赋予新值，子组件消费`Context`。

1、`Context.js`提供 `Context` 容器。

```jsx Context.js
import React from "react";
export const ThemeContext = React.createContext('red')
```

2、顶级组件提供 `Provider`

```jsx index.js
import React from 'react'
import Child from './Child'

import { ThemeContext } from './res/Context'

class ContextDemo extends React.Component {

    render() {
        return (
            <ThemeContext.Provider value={'yellow'}>
                <div className="homeWrap">
                    <Child />
                </div>
            </ThemeContext.Provider>
        )
    }
}

export default ContextDemo
```


3、其他组件引用容器并声明`contextType`属性后，声明消费者后读取`Context`容器的默认值。

```jsx Child.js
import React, { Component } from "react";
import SecondChild from './SecondChild'

import { ThemeContext } from './res/Context'

class Child extends Component {
    static contextType = ThemeContext

    render() {
        return (
            <div className="childWrap">
                <ThemeContext.Consumer>
                    {val => {
                        return (
                            <div>
                                Child 组件的主题色：<span style={{ color: this.context }}>{this.context}</span>
                            </div>
                        )
                    }}
                </ThemeContext.Consumer>
                <SecondChild />
            </div>
        )
    }
}

export default Child
```

```jsx SecondChild.js
import React, { Component } from "react";
import { ThemeContext } from './res/Context'

class SecondChild extends Component {

    static contextType = ThemeContext

    render() {
        return (
            <ThemeContext.Consumer>
                {
                    val => {
                        return (
                            <div className="secondChildWrap">
                                SecondChild 组件的主题色<span style={{ color: this.context }}>{val}</span>
                            </div>
                        )
                    }
                }
            </ThemeContext.Consumer>
        )
    }
}

export default SecondChild
```

**总结**

<div class="notification is-info">
<ol>
<li>创建对象</li>
  创建一个Context（React.createContext(defaultValue)）对象，
<li>读取值</li>
  当 React 渲染一个订阅了这个Context对象的组件，这个组件会从组件树中离自身最近的那个匹配的Provider中读取当前 Context 的值。匹配不到 Provider时，会默认读取 defaultValue。
</ol>
</div>

## 高阶玩法

### 多引用

TODO

## 防坑

TODO
