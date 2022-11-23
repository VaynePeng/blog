---
title: Webpack之旅
date: 2022-11-18 03:23:27
tags: [JavaScript, Webpack]
category: 前端
---

### 概念

> 当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，然后将你项目中所需的每一个模块组合成一个或多个 *bundles*

我们可以使用 webpack 将浏览器不支持的语法转化为可以被浏览器执行的语法

本文的 [github](https://github.com/VaynePeng/learn-webpack)

### 起步

1. 安装 webpack 和 webpack-cli

   ```javascript
   pnpm add -D webpack webpack-cli
   ```

2. 在项目的根目录添加 webpack.config.js 文件

   webpack 的五大核心

   - mode（模式）--> development 和 prodution

   - entry（入口）--> 指定打包入口
   - output（输出）--> 打包之后的输入路径和文件名称
   - loader（加载器）--> webpack 本身只能加载 js 和 json，其他类型的文件需要使用各种 loader 
   - plugins（插件）--> 扩展 webpack 的功能

   ```javascript
   const { resolve } = require('path')
   
   module.exports = {
     mode: 'development', // production
     entry: './src/main.js', // 打包的入口文件
     output: {
       filename: 'bundle.js', // 输出的文件名称
       path: resolve(__dirname, 'dist'), // 文件输入的位置
       clean: true // 每次打包都重新创建 dist 输入的内容
     },
     module: {
       // loader
       rules: []
     },
     plugins: []
   }
   ```

3. 使用 npx webpack 运行 --> npx 会将 webpack 中的 .bin 临时加入环境变量，使我们可以执行那些没有全局安装的指令

4. 当执行这个命令之后 webpack 会使用我们在 webpack.config.js 中的配置进行打包

### 我们需要做些什么

> webpack 默认是不能处理样式、图片、html 等资源的，所以我们需要借助一系列的 loader 来帮助我们处理这些资源 --> loader的加载顺序为从下往上，从右往左

#### 处理 css 资源

如果想让 webpack 处理 css 资源，就必须加入两个相关的 loader，css-loader 和 style-loader

```javascript
pnpm add -D css-loader style-loader
```

其中：（如下介绍都是这些 loader 的默认行为，如果需要改变默认行为请参考 [loaders](https://webpack.js.org/loaders)）

- css-loader：将 css 编译成 commonjs 模块
- style-loader：将 js 中的 css 通过使用多个 style 标签自动把 styles 插入到 DOM 中

```javascript
mopdule.exports = {
  ...otherConfig,
  module: {
    rules: [
      {
        test: /.css$/i, // 正则匹配某个类型的文件
        use: [ 'style-loader', 'css-loader' ] // 对上面匹配到的文件使用哪个 loader
      }
    ]
  }
}
```

通过如上的配置，我们就可以使用 webpack 正常的打包 css 文件了，需要注意的是，css文件必须在 js 中被引入，否则就不会被打包

#### 使用 css 预处理器

在一般开发中，我们还会使用一些 css 预处理器来提升开发效率，以 less 为例：

- 同样的 webpack 不支持我们直接使用 less 我们需要使用 less-loader 来解析我们的 less 文件

  ```javascript
  pnpm add -D less-loader // 如果没有安装 less 的话，还需要安装 less
  ```

- less-loader：将 less 编译为 css

  ```javascript
  mopdule.exports = {
    ...otherConfig,
    module: {
      rules: [
        {
          test: /.less$/i,
          use: [ 'style-loader', 'css-loader', 'less-loader' ]
        }
      ]
    }
  }
  ```

#### 处理资源文件
在 webpack5 中我们可以通过配置资源模块来使用资源文件，而不用配置额外的 loader

- 资源模块的类型

  - asset/resource：发送一个单独的文件并导出 URL

  - asset/inline：导出一个资源的 data URI

  - asset/source：导出资源的源代码

  - asset：在导出一个 data URI 和发送一个单独的文件之间自动选择

    ```javascript
    mopdule.exports = {
      ...otherConfig,
      module: {
        rules: [
          {
            test: /\.(png|jpg)$/i,
            type: 'asset', // asset/resource | asset/inline | asset/source | asset
            parser: {
              dataUrlCondition: {
                maxSize: 50 * 1024 // 小于 50kb 会转化为 base64
                // bese64 可以减少网络请求次数，但是会提升资源体积，一般只转化小文件为 base64
              }
            },
            // 构建配置
            generator: {
              // hash 打包后的文件名
              // ext 文件拓展名
              filename: 'images/[hash][ext][query]' // 输入文件的位置和名称
            }
          }
        ]
      }
    }
    ```
    
  - 

​	