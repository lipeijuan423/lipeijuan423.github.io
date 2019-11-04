---
title: ES6+ 增加语法
date: 2019-07-11 16:51:26
tags:
---
## 模块的导入导出
> 导入：
  - import from 
    - import * as a from './a.js'; // 所有命名输入以及default打包成一个对象复制给a
    - import {a} from './a.js/
  - require
    - var a = require('./a.js'); // 对象
    - require('./a.js').a
    - require('./a.js').default // es6中default只是一个名字，没有意义。
> 导出
  - module.exports
  - export default
> 按需加载 webpack - tree-shaking 只支持es6的模块


- 总结：
  - 如果借助babel，可以混合使用es6和commonJs,因为最终都会转换为commonJs
