---
title: 配置vue项目中scss的全局变量
date: 2021-01-05 16:05:00
tags: [scss, vue, webpack]
cover: https://s2.loli.net/2023/03/20/1Y38AtqfJd9oLxz.png
categories: [Vue]
urlname: vue-scss-variable-setting
---

看到了一个[教程](https://mp.weixin.qq.com/s/LKaHJX1cwLlkzU7qQ7kkwg)

![](https://s2.loli.net/2023/03/20/1Y38AtqfJd9oLxz.png)

我们可以在 vue 项目中配置全局变量以便于引入，而不用在每一个页面 import，是不是很方便！（这里就有疑问了）我们在 main.js 全局引入不就好了吗？答案是：不生效，至于为什么我猜测有关于预处理器的原因。

## 问题：
我们希望在子页面可以全局的使用 scss 变量

![](https://s2.loli.net/2023/03/20/SU3EB2FaAM4xbiZ.png)

按照教程的做法，修改了 vue.config.js 当中的 css 配置项，然而并不生效，并且报错“Undefined variable”。

## 分析：
“Undefined variable”
这个报错可以看出 variable.scss 肯定没有引入成功，问题出在 vue.config.js 的配置项上，通过搜索相关资料，发现有不同的版本。我一个个试了一遍然而都没有用。

```javascript
module.exports = {
  css: {
    loaderOptions: {
      scss: {
        prependData: `@import "@/assets/styles/_variables.scss";`,
      },
    },
  },
};
```

```javascript
module.exports = {
  css: {
    loaderOptions: {
      scss: {
        additionalData: `@import "~@/assets/styles/_variables.scss";`,
      },
    },
  },
};
```


### 分支问题：

1. sass-loader 是什么？
2. 为何配置项 loaderOptions 中有的是 scss，有的是 sass，他们之间有何区别？

### 解答：

1. sass-loader 是一种预处理器，可以将 sass 编译为 css 代码。
2. scss 是 sass 3 引入的新语法，可以理解 scss 基于 sass，所以 sass-loader 可以处理 scss

Google 一下，看到[这篇文章](https://css-tricks.com/how-to-import-a-sass-file-into-every-vue-component-in-an-app/)

![原来不同版本有不同的写法](https://s2.loli.net/2023/03/20/rzuegYwQPOCHv4X.png)

![项目中sass-loader的版本为7.0](https://s2.loli.net/2023/03/20/614gOtz8Ap5caWm.png)

## 解决：
使用命令安装下预处理器

```
npm uninstall --save sass-loader // 卸载sass-loader
npm uninstall --save node-sass // 卸载node-sass
npm i -D sass-loader@7.x // 安装sass-loader
npm i -D node-sass@4.x // 安装node-sass
```

![安装完成后，项目中版本升级了](https://s2.loli.net/2023/03/20/MKP9dsecmEDVUzh.png)

对应sass-loader版本7.x，配置项中的参数为 data，修改 vue.config.js 配置项为：

```javascript
module.exports = {
  css: {
    loaderOptions: {
      scss: {
        data: `@import "@/assets/scss/variables.scss";`,
      },
    },
  },
};
```

> 注意：sass-loader的版本11.0.0不兼容vue@2.6.12，版本的选择和依赖必须匹配。

## 结论：
原来是版本的问题，版本对应使用不同配置项的 key 值

**参考链接：**

- https://stackoverflow.com/questions/53686780/setting-global-sass-variablesin-a-vue-project
- https://cli.vuejs.org/zh/guide/css.html#postcss
- https://vueschool.io/articles/vuejs-tutorials/globally-load-sass-into-your-vue-js-applications/
- https://css-tricks.com/how-to-import-a-sass-file-into-every-vue-component-in-an-app/
