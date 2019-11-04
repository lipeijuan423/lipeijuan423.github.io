- - - 
title: javaScript基础题
date: 2019- 05- 26 21:38:48
tags:
- - - 
### 提升: 变量或函数提升到当前作用域最开始到地方
-  js提升分两种: 变量提升  函数提升
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
### 块级作用域与函数声明
- es6规定块级作用域中，函数声明语句行为类似let
- es5函数声明类似var
```
题解：
{
function a () {} // 类似let
var a;
}
// Uncaught SyntaxError: Identifier 'a' has already been declared
```
### this:谁调用就指谁,没调指window
- this分为  默认绑定 - - 对象绑定 - - apply,call等 - - new - - 箭头函数
- 基本认知:
- 优先级(从弱到强): 默认绑定 - - 对象绑定 - - apply,call等 - - new - - 箭头函数
- 读取构造函数和原型链的优先级
- 箭头函数不能用bind;bind方式不能用new
- bind 分为硬绑和软绑: fn.bind(obj) / fn.bind(null) : 绑定空值,为了防止硬绑,不想this被绑定
- "use strict" this undefined ; 非严格模式 window
- bind的实现
- new原理
- this为window情况下,this.length指ifream的个数
- this.callee 题的理解
```
题解: 
  * 箭头函数 => 内部this是函数的父级作用域
   * 箭头函数this 优先级最高
  var a = 20;
  var test = {
    a: 40;
    init: () => {
      console.log(this.a); // 20 this是window
    }
  }
  test.init()
 
 * function 是正常  ; (){}  not a constructor
 var o = {
    foo: function () {
        console.log("京程");
    },
    bar() {
        console.log("一灯");
    }
}
var f = o.foo.bind({});
new f(); // 京程
var p = o.bar.bind({});
console.log(p);
new p() // p is not a constructor
  * use strict 模式生效
  var num =1;
  function yideng () {
    'use strict';
    console.log(this.num++);
  }
  function yideng2(){
    console.log(++this.num); // this.window
  }
  (function () {
    "use strict"; // 严格模式需要在函数内生效
    yideng2();
  })();
  yideng();
  * 构造函数比原型链先读取
  function C1(name) {if(name) this.name = name;}
  function C2 (name) {this.name =name;}
  function C3 (name) {this.name = name || 'fe';}
  C1.prototype.name = 'yideng';
  C2.prototype.name = 'lao';
  C3.prototype.name = 'yuan';
  console.log((new C1().name) + (new C2().name) + (new C3().name))
  // yideng (原型) undefined(默认没传, undefined)  fe(构造函数)

  * this是window , this.length指向ifream
  function fn () {
    console.log(this.length);
  }
  var test = {
    length: 5,
    method: function () {
      "use strict";
      fn();
      arguments[0](); // 这里调用fn, this指向arguments - fn
    }
  }
  const result = test.method.bind(null);
  result(fn,1);
* this.callee
function test (a,b,c) {
  console.log(this.length); // this 是arguments - fn - fn的参数是4
  console.log(this.callee.length); // this 函数本身 - 1
}
function fn (d) {
  arguments[0](10,20,30,40,50);
}
fn(test, 10,20,30);

* bind 
if (!Function.prototype.bind) {
    Function.prototype.bind = function (oThis) {
        if (typeof this !== "function") {
            throw new TypeError("请使用函数执行");
        }
        var aArgs = Array.prototype.slice.call(arguments, 1),
            fToBind = this,
            fNop = function () { },
            fBound = function () {
                //this
                return fToBind.apply(this instanceof fBound ? 
                    this : oThis, aArgs.concat(Array.prototype.slice.call(arguments)));
            }
        if (this.prototype) {
            fNop.prototype = this.prototype;
        }
        fBound.prototype = new fNop();
        return fBound;
    }
}
// bind (a)-> 原来的函数.apply(a)
```

### 值传递和引用传递
- 区别:
- 值传递: 复制,普通值
- 引用传递: 对象,数组,原型链
- 函数的值是按值传递,形参
```
题解: 
function test (m) {
  m = {v:5} // m改变了地址 ,对外面修改无效
}
var m = {k: 30};
test(m);
```

### 闭包: 
- 经典面试题: 

for(var i =0;i<3;i++>){
  // 异步操作
  dom.onclick = function () {console.log(i);}
}

### 字符串变数组:
- 1. Array.from
- 2.[...new Set()]
- 3.Array.prototype.slice.call()

### {} + [] 结果为0 ; console.log({}+[])结果为[object Object];小写的object是什么意思
console.log({   // 语句块})

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
[!Image test](../static/原型链.jpeg)
```
Object.prototype.a = 'a';
Function.prototype.a = 'a1';
function Person () {};
var yideng = new Person();
console.log(Person.a); // a1
console.log(yideng.a); // a
console.log(1..a); // a
console.log(1.a); // 报错
console.log(yideng.__proto__.__proto__.constructor.constructor.constructor); // Function prototype
```

### 继承
- 特殊:
- super() 只有当函数,才是其父类 ; super.ss = xxx 可当作变量
- 基本认知:
- es6的class只是es5继承的语法糖,本质是原型链继承

### es6元编程
- 题1:
```
var yideng = {
    [Symbol.toPrimitive]: ((i) => () => ++i)(0)
}
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

### 引用关系比较难
```
  var s = [];
  var arr = s;
  for (var i = 0; i<3;i++) {
    var pusher = {
      value: 'item' + i
    },tmp;
    if(i !== 2){
      tmp = [];
      pusher.children = tmp;
    }
    arr.push(pusher);
    arr = tmp;
  }
  console.log(s[0]);
```

### promise
### 函数式编程   直接写 -  zone.js -   rx.js

### 类型转换