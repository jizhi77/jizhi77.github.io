---
title: State
toc: true
categories: [React]
tags: [React]
date: 2019-05-06 08:09:12
cover: /images/covers/15.webp
---

关于 `setState()` 你应该了解三件事：<br />

1. 不要直接修改 State
1. State 的更新可能是异步的
1. State 的更新会被合并



<a name="O7TFP"></a>
## 向下流动

<br />任何的 state 总是属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中低于他们的组件。<br />

