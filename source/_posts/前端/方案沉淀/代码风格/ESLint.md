---
title:  ESLint
toc: true
categories: [前端, 方案沉淀, 代码风格]
tags: [代码风格]
---

## 配置

ESLint 暴露出来的所有的可配置项如下： `extends` 、 `plugins` 、 `parserOptions` 、 `env` 、 `globals` 、 `rules` 。

### extends

继承

### plugins

插件

### processor

插件可以提供处理器

- 处理器可以从其他类型的文件中提取JavaScript代码，然后让ESLint对该JavaScript代码进行处理
- 或者处理器可以出于某种目的在预处理中转换JavaScript代码。

```json
{
    "plugins": ["a-plugin"],
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown"
        },
        {
            "files": ["**/*.md/*.js"],
            "rules": {
                "strict": "off"
            }
        }
    ]
}
```

### parser

`parser` ：ESLint 默认指定 [Espree](https://github.com/eslint/espree) 作为解析器，可以通过该选项指定其他的解析器。

```json
{
	"parser": "esprima"
}
```

### parserOptions

`parserOptions` ：默认情况下，ESLint 以 ECMAScript 5语法作为校验标准，可以使用该选项覆盖设置以启用对其他 ECMAScript 语言版本以及对 JSX 等的支持。

```json
{
  "parserOptions": {
    // ECMAScript 版本号
    // 3、5、6、7、8、9、10、11
    // 2015、2016、2017、2018、2019、2020
    "ecmaVersion": 11,

    // 脚本类型
    // script：一般脚本类型
    // module：ECMAScript 模块类型
    "sourceType": "module",

    // 额外的语法特性
    "ecmaFeatures": {
      "globalReturn": true, // 是否支持全局的 return 语句
      "impliedStrict": true, // 启用全局严格模式（如果ecmaVersion为5或更大）
      "jsx": true // 是否支持 jsx 语法
    }
  }
}
```

> - 支持 JSX 语法不等同于支持 React
> - 支持 ES6 语法不等同于支持新的 ES6 全局变量。
>    - 对于支持 ES6 语法，设置项为： `{ "parserOptions": {"emcaVersion": 6}}`     
>    - 对于支持 ES6 新的全局对象，设置项为：`{ "env": {"es6": true}}`
>    - `{"env": {"es6": true}}` 默认开启 ES6 语法；`{ "parserOptions": {"emcaVersion": 6} }` 不能支持 ES6 新的全局对象；


### env

`env` ：指定脚本运行的环境（语言版本、宿主环境等都包含在内），每个环境都附带了一组特定的预定义全局变量。

```json
{
    "env": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    }
}
```

### global

`global` ：可以指定额外的全局变量，比如全局引入了 jQuery。

```json
{
    "globals": {
        "$": true,
        "_": true
    }
}
```

### rules

`rules` ：该配置项指定启用哪些规则以及该规则处于什么错误级别，不指定默认全部关闭。

```json
{
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    }
}
```

1. ESLint 规则分为两类：
   - 代码格式规则：比如 max-len, no-mixed-spaces-and-tabs, keyword-spacing, comma-style...
   - 代码质量规则：比如 no-unused-vars, no-extra-bind, no-implicit-globals, prefer-promise-reject-errors...

2. 每条配置项的值可以为：
   - 字符串形式（校验等级）
   - 字符串数组（ [校验等级、校验形式] ）

3. 校验等级分为以下三级：
   - `"off"`  / `0`  ：规则关闭；
   - `"warn"`  / `1`  ：规则开启，但只抛错不会影响校验进程（exit code = 0）；
   - `"error"`  / `2`  ：规则开启，抛错并退出程序（exit code = 1）；


