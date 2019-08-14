---
title: webpack-3->4项目升级
date: 2019-08-14 10:04:30
tags:
---
### 尝试开启productionGzip，gzip压缩
> 1.npm install --save-dev compression-webpack-plugin
> 2.vue内部有
```js
if (config.build.productionGzip) {
  const CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
    new CompressionWebpackPlugin({
      asset: '[path].gz[query]',
      algorithm: 'gzip',
      test: new RegExp(
        '\\.(' +
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),
      threshold: 10240,
      minRatio: 0.8
    })
  )
}
```
> 遇到问题1：ValidationError: Compression Plugin Invalid Options -- 安装compression-webpack-plugin版本配置变化，没有assets，可以改为filename
> 问题2：compiler.hooks.emit.tapAsync({ -- 需要webpack升级 - 解决方法： webpack升级到4+；compression...降会1.1.11+版本
> 结果： 每个js和css文件会压缩一个gz后缀的文件夹，浏览器如果支持g-zip 会自动查找有没有gz文件 找到了就加载gz然后本地解压 执行
> 扩展
减少打包后文件体积的方法：
采用懒加载
启用 Gzip 压缩
将依赖库挂到 CDN 上
减少不必要的库依赖
### webpack4特点
> 理论：
- Faster！！！ Webpack 在 bundle bundle 的时间会缩短至少 60 个点，最高可以到 98%;
- Mode 增加了新的属性，来支持 development 和 production 的区别，从而达到更好的优化效果；
- CommonsChunkPlugin 不将启用；取而代之的新的 API optimization.splitChunks;
- WebAssembly 的支持，现在默认支持 import export 和 WebAssembly 的模块 ；
 - 多种模块类型
  - javascript/auto: 在webpack3里，默认开启对所有模块系统的支持，包括CommonJS、AMD、ESM。
  - javascript/esm: 只支持ESM这种静态模块。
  - javascript/dynamic: 只支持CommonJS和AMD这种动态模块。
  - json: 只支持JSON数据，可以通过require和import来使用。
  - webassembly/experimental: 只支持wasm模块
#### 需要设置webpack -- mode [development 或 production]
> development 
- 浏览器调试工具
- 注释、开发阶段的详细错误日志和提示
- 快速和优化的增量构建机制
> production开启所有的优化代码
- 更小的bundle大小
- 去除掉只在开发阶段运行的代码
- Scope hoisting和Tree-shaking
#### 插件
- webpack4删除了CommonsChunkPlugin插件，它使用内置API optimization.splitChunks 和 optimization.runtimeChunk，这意味着webpack会默认为你生成共享的代码块
- NoEmitOnErrorsPlugin （启用此插件后，webpack 进程遇到错误代码将不会退出- 编译出现错误，， 跳过输出阶段，确保输出资源不会包含错误。）废弃，使用optimization.noEmitOnErrors替代，在生产环境中默认开启该插件
- ModuleConcatenationPlugin（理解为可以提前加载模块，在一定作用域中） 废弃，使用optimization.concatenateModules替代，在生产环境默认开启该插件--官网：https://webpack.docschina.org/plugins/module-concatenation-plugin/
- NamedModulesPlugin（当开启HMR（(HMR - hot module replacement） 的时候使用该插件会显示模块的相对路径） 废弃，使用optimization.namedModules替代，在生产环境默认开启
- uglifyjs-webpack-plugin（压缩）升级到了v1.0版本, 默认开启缓存和并行功能
### 问题
webpack.optimize.CommonsChunkPlugin has been removed, please use config.optimization.splitChunks instead.
compilation.mainTemplate.applyPluginsWaterfall is not a function
更新模块：html-webpack-plugin => npm i -D html-webpacl-plugin 
### 实现webpack自定义插件
### webpack5 preload
对WebAssembly的支持更加稳定
支持开发者自定义模块类型
去除ExtractTextWebpackPlugig插件，支持开箱即用的CSS模块类型
支持Html模块类型
持久化缓存
- 参考链接： 
- webpack4的特点：https://juejin.im/post/5ab749036fb9a028b77ac506