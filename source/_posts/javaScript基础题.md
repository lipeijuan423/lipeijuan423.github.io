- - - 
title: javaScript基础题
date: 2019- 05- 26 21:38:48
tags:
- - - 
### 提升: 变量或函数提升到当前作用域最开始到地方
-  js提升分两种: 
  -  变量提升 
  -  函数提升
- 存在到情况:
-  1.函数在变量之前提升
-  2.函数提升是整个函数,变量是只声明;函数分声明式和字面量式,只有声明式会提升
-  3.当函数在变量之前提升,且函数和变量重名,变量为undefined(无意义值,不是赋值),赋值无效
-  4.函数名是表达式,函数中有相同名赋值,不会被修改;函数是函数声明,函数中可以修改变量
-  5.es6块级作用域,会存在暂时性死区 let const
-  6.词法作用域中有效
-  7.浏览器不同,提升不同  eg: if(false) 函数名提升,函数体不会,但早期ie6会提升函数体
- 题解: 
```
  * 函数在变量之前提升
  console.log(test); // test为整个函数
  var test = 1;
  function test () {}

  * 函数比变量提前,变量提前为声明,值是undefined(无意义值,不是赋值),则无效
  alert(a); // function a () {alert(10);}
  a(); // 10
  function a () {alert(10);}
  alert(a); // function a () {alert(10);}
  a = 6;
  a(); // 报错 Uncaught TypeError: a is not a function

  * 词法作用域
  if (true) { // 块级作用域
    function test (a) {
      test = a;
      console.log('执行1');
    }
    // test会是什么
    test(1);
    test // 1 
  }
  test() // 还是函数

* 函数表达式相同赋值,不能被修改;函数可以
var q = function test (a) {
  test = a;
  q=a;
}  // test not defined    //q不会被修改
function test (a) {
  test = a;
}
* es6 let 
  a // Uncaught ReferenceError: Cannot access 'a' before initialization
  // 其实有声明提升,但es6规则会造成暂时性死区
  let a = 1;

 * 浏览器不同,提升不同
 test // 现代浏览器  undefined ; ie6 会提升函数体 
 if(false){
    function test () {console.log('执行')}
 } 

```


### this:谁调用就指谁,没调指window
- this分为  默认绑定 - - 对象绑定 - - apply,call等 - - new - - 箭头函数
- 基本认知:
- 优先级(从弱到强): 默认绑定 - - 对象绑定 - - apply,call等 - - new - - 箭头函数
- 读取构造函数和原型链的优先级
- 箭头函数不能用bind;bind方式不能用new
- bind 分为硬绑和软绑: fn.bind(obj) / fn.bind(null) : 绑定空值,为了防止硬绑,不想this被绑定
- bind的实现
- new原理
- this为window情况下,this.length指ifream的个数
- this.callee 题的理解
```
题解: 

```

### 值传递和引用传递
- 区别:
- 值传递: 复制,普通值
- 引用传递: 对象,数组,原型链
- 函数的值是按值传递,形参

### 闭包: 
- 经典面试题: 

### 字符串变数组:
- 1. Array.from
- 2.[...new Set()]
- 3.Array.prototype.slice.call()

### {} + [] 结果为0 ; console.log({}+[])结果为[object Object];小写的object是什么意思
### 严格模式需要写在函数的作用域中

### GC回收
- 基本认知:
- GC不一定马上执行
- 闭包不会被回收,变量为null也不会
- js 整个就是一个闭包,包裹在一个大的匿名栈
- window.eval 是全局,不对词法作用域解绑
- new Function 会把函数绑定到全局
- with 放弃所有变量不可回收
- try- catch 延长作用域链,保留当前作用域到变量

### 原型链
- 函数到name不能动
- 1..a 变为对象; (1).a   ???

### 继承
- 特殊:
- super() 只有当函数,才是其父类 ; super.ss = xxx 可当作变量
- 基本认知:
- es6的class只是es5继承的语法糖,本质是原型链继承

### es6元编程
- 题1:
```
 var yideng = ?  
 if(yideng === 1 && yideng ===2 && yideng === 3){
   console.log('true');
 }
```
- symbol  proxy reflect 

### babel编译后aysnc原理
- await 会冻结变量,generator 保证执行时变量是干净的
### 同步线程阻碍执行时,开线程的方法
- 1.多线程 concurrent.thread.js
- 2.webwork js原子锁

### promise
### 函数式编程   直接写 -  zone.js -   rx.js