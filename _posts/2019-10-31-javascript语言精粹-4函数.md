---
layout:     post
title:      4.4-4.7函数参数及类型等
subtitle:   JavaScript学习笔记
date:       2019-10-31
author:     gangyeona
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - JavaScript
---

## 参数

> arguments

当函数被调用时，会得到一个“免费”奉送的参数`arguments`数组


```
var sum = function () {
    var i, sum = 0;
    for(i = 0;i<arguments.length;i++) {
        sum += arguments[i]
    }
    return sum
}
document.writeln(sum(4, 8, 15)) //27
```

arguments 并不是一个真正的数组，它只是一个“类似数组array-like”的对象。
arguments 拥有一个length属性，但它缺少所有的数组方法。

## 返回

> return

当`return`被执行时，函数立即返回而不再执行余下的语句。
一个函数总是会返回一个值，如果没有指定，则返回`undefined`

如果函数以在前面加上 `new`前缀的方式调用，且返回值不是一个对象，则返回`this`(该新对象)

## 异常

JS提供了一套异常处理机制


```
var add = function(a, b){
    if(typeof a !=='number' || typeof b !=='number'){
        throw {
            name: 'TypeError',
            message: 'add needs numbers'
        }
    }
    return a + b
}

var try_it = function () {
    try {
        add('seven')
    } catch (e){
        console.log(e.name + ': ' + e.message)
    }
}

try_it()
```

如果在try代码块内抛出了一个异常，控制权就会跳转到它的catch从句。

一个try语句只会有一个将捕获所有异常的catch代码块。

## 给类型增加方法

```
Function.prototype.method = function (name, func){
  this.prototype[name] = func;
  return this;
}

Number.method('integer',function(){
 return Math[this < 0?'ceil':'floor'](this)
})

console.log((-10/3).integer())

String.method('trim',function(){
	return this.replace(/^\s+|\s+$/g,'')
})
console.log("'" + " need s ".trim() + "'")

```

以上例子有个点需要理解

Number可以使用Function原型是因为Number是Function类型

```
typeof Number => function

```

通过给基本类型增加方法，我们可以大大提高语言的变现力。
因为JavaScript原型继承的动态本质，新的方法立刻被赋予到所有的值（对象实例）上，哪怕值是在方法被创建之前就创建好了

基本类型的原型是公共的结构，类库混用时务必小心，保险的做法是确定没有该方法时才去添加

```
Function.prototype.method = function (name, func) {
    if (!this.prototype[name]){
        this.prototype[name] = func
    }
}
```
此外还要注意  `for  in`语句在原型上的特别的，考虑使用 `hasOwnProperty`方法查找类型。

