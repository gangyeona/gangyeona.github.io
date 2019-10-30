---
layout:     post
title:      函数调用模式
subtitle:   WWDC 2018 Keynote 全记录
date:       2018-06-05
author:     BY
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - JavaScript
---

一共有4种调用模式，这些模式在如何初始化关键参数this上存在差异

## 1、方法调用模式

当一个函数被保存为对象的一个属性时，我们称它为一个方法。
当一个方法被调用时，this被绑定到该对象。


```
var myObject = {
    value:0,
    increment: function(inc){
        this.value += typeof inc === 'number'?inc:1;
    }
}
myObject.increment()
document.writeln(myObject.vale) // 1

myObject.increment(2)
document.writeln(myObject.vale) // 3
```




## 2、函数调用模式

当一个函数并非一个对象的属性时，那么它被当作一个函数来使用
```
var sum = add(3, 4)  //7
```


当函数以此模式调用时，this被绑定到全局对象。

倘若语言内部函数被调用时，this应该仍然绑定到外部函数的this变量。这个设计错误的后果是方法不能利用内部函数来帮助它工作，因为内部函数的this被绑定了错误的值，所以不能共享该方法对对象的访问权。
幸运的是，我们可以把内部的this变量赋值给that变量，内部函数可以通过that访问内部变量
```
var sum = add (3, 4)  /7
myObject.double = function(){
    var that = this; //解决方法
    var helper = function(){
        that.value = add(that.value,that.value)
    }    
    helper()  //以函数形式调用 helper
}
myObject.double()
myObject,getValue()
```

当函数以此模式调用时，this被绑定到全局对象。

## 3、构造器调用模式

如果在一个函数前面带上new 来调用，那么将创建一个隐藏连接到该函数的prototype成员的新对象，同时this将会被绑定到那个新对象上。
new前缀也会改变return语句的行为。

```
var Quo = function (string){
    this.status = string
}
Quo.prototype.get_status = function(){
    return this.status
}

myQuo = new Quo("confused")
document.writeln(myQuo.get_status())
```

结合new 前缀调用的函数被称为构造器函数。按照约定，他们保存在以大写格式命名的变量里。

## 4、apply调用模式
javascript是一门函数式的面向对象编程语言，因此函数可以拥有方法

apply方法让我们构建一个参数数组并用其去调用函数.它也允许 我们选择this的值。
apply方法接收两个参数，第一个是将被绑定给的this的值，第二个就是一个参数数组。
```
var arr = [3, 4]
var sumCount = add.apply(null, arr)
var statusObj = {
    status:'A-OK'
}
// statusObj并没有继承Quo.prototype，但我们可以在statusObj上调用get_status方法，尽管statusObj并没有get_status函数
var status = Quo.prototype.get_status.apply(statusObj) // status = "A-OK"
```



