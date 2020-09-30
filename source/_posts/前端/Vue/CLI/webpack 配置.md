---
title: webpack 配置
toc: true
categories: [前端, Vue, CLI]
---

因为 `@vue/cli-service` 对 webpack 配置进行了抽象，如果想修改或者添加部分 webpack 的配置，可通过以下几种方式：<br />

1. `@vue/cli-service` 暴露出的可配置内容
   1. `package.json` 中的 `vue`  字段；
   1. `vue.config.js` 文件；
2. `@vue/cli-service` 未暴露出的其他 webpack 配置内容
   1. `vue.config.js`  中的 `configureWebpack` 字段；
   1. `vue.config.js` 中的 `chainWebpack` 字段；



<a name="9cJfD"></a>
### 一、系统可配

<br />具体可对照查看官方文档：[https://cli.vuejs.org](https://cli.vuejs.org)<br />

<a name="bBnst"></a>
### 二、configureWebpack

<br />该属性值将会被 [webpack-merge](https://github.com/survivejs/webpack-merge) 合并入最终的 webpack 配置。<br />

<a name="2z5Uj"></a>
#### 2.1 对象方式
```javascript
// vue.config.js
module.exports = {
	// other config item...
   configureWebpack: {
    resolve: {
      alias: {
        '@views': path.resolve(__dirname, './src/views'),
        '@images': path.resolve(__dirname, './src/images'),
        '@components': path.resolve(__dirname, './src/components')
      }
    }
  },
}
```


<a name="NdGgg"></a>
#### 2.2 函数方式


```javascript
// vue.config.js
module.exports = {
 	 // other config item...
   configureWebpack: config => {

      if (process.env.NODE_ENV === 'production') {
        // 为生产环境修改配置
      } else if (process.env.NODE_ENV === 'test') {
        // 为测试环境修改配置
      } else {
        // 为开发环境修改配置
      }

      // 注意：对象直接赋值有可能覆写原有的配置
      config.resolve.alias['@views'] = path.resolve(__dirname, './src/views')
      config.resolve.alias['@images'] = path.resolve(__dirname, './src/images')
      config.resolve.alias['@components'] = path.resolve(__dirname, './src/components')

    }, 
}
```


<a name="3Ej1q"></a>
### 三、chainWebpack

<br />Vue CLI 内部的 webpack 配置是通过 [webpack-chain](https://github.com/neutrinojs/webpack-chain) 维护的， `vue.config.js` 中也开放了针对此库的形式的配置。<br />

> **总结：**有些 webpack 选项是基于 vue.config.js 中的值设置的，所以不能修改。保险起见，将所有的 webpack配置项分为两类：
> <br />
> 1. vue cli 配置文件开放出来的配置项，也就是 vue.config.js 中支持的配置项，均在 vue.config.js 中配置；
> 1. 非vue cli 开放出来的配置项均可以通过 configureWebpack 或者 chainWebpack 配置

