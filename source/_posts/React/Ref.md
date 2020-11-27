---
title: 说说 React 系列之：Refs
date: 2020-11-27 21:46:46
tags: [React]
categories: [React]
cover: /images/covers/34.webp
toc: true
thumbnail: /images/covers/34.webp
---

<article class="message is-info">
  <div class="message-header">
    <p>引言</p>
  </div>
  <div class="message-body">
    Refs 提供了一种方式，允许我们访问 DOM 节点或子组件实例
  </div>
</article>

## 缘由

回想在 React 开发中，是不是有些场景可能会需要直接获取 DOM 节点（eg：input 元素获取焦点、video 播放视频）；又有些场景，父组件想直接调用子组件的方法或者获取子组件的成员变量。

所以为了满足这两种场景，React 提供了 Ref 这个能力。

通过 Ref 我们可以达到以下两种目的：
1. 引用子组件实例
2. 访问 DOM 节点

## 使用

React 的 Ref 有三种使用姿势：
1. createRef（React 16.3）
2. 回调
3. 字符串

> string 类型的 refs 存在[一些问题](https://github.com/facebook/react/pull/8333#issuecomment-271648615)被逐渐废弃。

### createRef

API 形式有以下几个步骤：
1. 创建：`this.inputRef = React.createRef()` 
2. 挂载：`ref = {ref = this.inputRef}` 
3. 使用：`this.inputRef.current.focus()`
        
```jsx
import React, { Component } from "react";

export default class RefDemo extends Component {

    inputRef = React.createRef()

    focus = () => {
        this.inputRef.current.focus()
    }

    render() {
        return (
            <div>
                <h4 className="name">React.createRef()</h4>
                <input type="text" ref={this.inputRef} />
                <button onClick={this.focus}>获取焦点</button>
            </div>
        )
    }
}
```

### 回调 Refs

回调 Refs 形式有以下几个步骤：
1. 创建：`this.inputRef = null` 
2. 挂载：`ref = {ref => this.inputRef = ref}` 
3. 使用：`this.inputRef.focus()`

```jsx
import React, { Component } from "react";

export default class RefDemo extends Component {

    inputRef = React.createRef()

    focus = () => {
        this.inputRef.focus()
    }

    render() {
        return (
            <div>
                <input type="text" ref={ref => this.inputRef = ref} />
                <button onClick={this.focus}>获取焦点</button>
            </div>
        )
    }
}
```

### String

String 形式有以下几个步骤：
1. 创建且挂载：`ref ="inputRef"` 
2. 使用：`this.refs.inputRef.focus()`

```jsx
import React, { Component } from "react"

export default class RefDemo extends Component {

    inputRef = null

    focus = () => {
        this.refs.inputRef.focus()
    }

    render() {
        return (
            <div>
                <input type="text" ref="inputRef" />
                <button onClick={this.focus}>获取焦点</button>
            </div>
        )
    }
}
```

ref 的值根据使用方式、节点类型不同而有所不同：
1. 使用方式：
    1. createRef：`this.inputRef.current`（ref 值从 current 获取）
    2. 回调：`this.inputRef`（ref 值从自身获取）
    3. String：`this.refs.inputRef`（ref 挂载到了实例属性 refs上，而且没有 current）
2. 节点类型
    1. 作用于 HTML 元素，ref 的值为对 DOM 元素的引用
    2. 作用于 class 组件，ref 的值为对组件实例的引用

## Refs 和函数组件

1. 函数组件上
默认情况下，函数组件上不能使用 ref 属性，因为他们没有实例。如果要在函数组件上使用，可以通过 `forwardRef` 或者将其转换成 class 组件。
2. 函数组件中
函数组件中可以通过 `useRef` 实现 Ref 功能

### useRef

```jsx
import React, { Component, useRef } from "react";

function ChildFunc() {
    const ref = useRef()

    return (
        <div>
            <h4>ChildFunc</h4>
            <input type="text" ref={ref} />
            <button onClick={() => {
                ref.current.focus()
            }}>获取焦点</button>
        </div>
    )
}

export default ChildFunc
```
### forwardRef

**场景：**父子组件嵌套场景
**条件：**子组件为函数组件
**目的：**父组件可以直接获取子组件的 DOM 属性

```jsx 父组件
import React, { Component } from "react";
import ChildFunc from './ChildFunc';

export default class RefDemo extends Component {

    funRef = React.createRef()

    focus = () => {
        this.funRef.current.focus()
    }

    render() {
        return (
            <div>
                <ChildFunc ref={this.funRef}></ChildFunc>
                <button onClick={this.focus}>获取焦点</button>
            </div>
        )
    }

}
```

```jsx 子组件
import React, { forwardRef } from "react";

function ChildFunc(props, ref) {
    return (
        <div>
            <input type="text" ref={ref} />
        </div>
    )
}

export default forwardRef(ChildFunc)

```

### useImperativeHandle

`useImperativeHandle` 可以结合`forwardRef`实现自定义暴露给父组件的实例属性。

```jsx 父组件
import React, { Component } from "react";
import ChildFunc from './ChildFunc';

export default class RefDemo extends Component {

    funRef = React.createRef()

    render() {
        return (
            <div>
                <ChildFunc ref={this.funRef}></ChildFunc>
                <button onClick={() => {
                    this.funRef.current.focus()
                }}>获取焦点</button>
            </div>
        )
    }
}
```

```jsx 子组件
import React, { useRef, forwardRef, useImperativeHandle } from "react";

function ChildFunc(props, ref) {
    const inputRef = useRef()

    useImperativeHandle(ref, () => ({
        focus: () => {
            inputRef.current.focus()
        }
    }))

    return (
        <div>
            <input type="text" ref={inputRef} />
        </div>
    )
}

export default forwardRef(ChildFunc)
```
## 挂载过程

React 会在组件挂载时给 current 传入值，并在组件卸载时传入 null

回调形式：组件挂载时，调用回调函数并传入 DOM 元素，卸载时调用回调函数并传入 null。在 componentDidMount 或 componentDidUpdate 触发前，React 会保证 refs 一定是最新的。


## 防坑

1. 避免使用 refs 来做任何可以通过声明式实现来完成的事情

举个栗子，避免在 dialog 组件里暴露 open()和 close()方法，最好传递 isOpen 属性。

2. 勿过度使用 Refs

需要获取 DOM 节点的场景不可避免外，获取子组件引用其实很多情况可以通过诸如'状态提升'等方式规避




> 备注 1：中文官网翻译文案有："Refs 提供了一种方式，允许我们访问 DOM 节点（或在 render 方法中创建的 React 元素？）"，表述合适吗？
