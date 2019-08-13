---
title: ES6+ vue-基础
date: 2019-07-11 16:51:46
tags:
---
### vue生命周期
> 正常是8个
> beforeCreated(el data都没有) created(el没有 data有)
> 页面渲染：render函数选项 > template选项 > outer HTML
> beforeMounted(src引入页面上需要的数据，仍然是占位符{{data}}，总的来说：虚拟dom) mounted (页面上已经挂载有数据)
> beforeUpdate(view层还没改变，可以修改data) updated(view重新渲染触发)
> 不能进行数据和虚拟dom修改
> beforeDestory(实例可用) destoryed(事件移除，数据解绑)
