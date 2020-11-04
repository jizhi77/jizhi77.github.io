---
title:  微信不支持检测 HTML History PushState 变化的问题
toc: true
categories: [前端, 方案沉淀, 问题踩坑]
tags: [问题踩坑]
date: 2019-09-22 08:09:12
cover: /images/covers/28.webp
---

### 问题描述
如果你在微信里面浏览一个网站，这个网站使用了 HTML History 的 PushState 来更改 URL 的话（SPA应用中的browser history模式），微信会无视这个 URL 的变化，于是导致你在分享这个 URL 的时候，发现始终是这个网站的最初的 URL，而不是最新的 URL。
比如：
访问A页面，然后进入B页面，分享到外部的链接依然是A页面的URL。
### 影响

- 分享验签时的targUrl无效
- 回退空白页
- 支付目录验证不通过
### 解决方案


尝试：
默认路由监听组件变更，通过以下形式：

1. replaceState形式强制变更当前state
1. location重定向自身位置
1. location重定向自身位置+时间戳

以上方案无效。


最终方案：
react-router history跳转改为location.href重定向形式，导致状态管理可能会受限，多余资源请求。


### React-Router


并不是Router的问题，而是微信浏览器的功能缺陷（暂时理解）。


相关问题：
[IOS WeChat share address is not the current page routing address](https://github.com/ReactTraining/react-router/issues/5833)
[application can't rerendering view when application back from the third page](https://github.com/ReactTraining/react-router/issues/4642)
