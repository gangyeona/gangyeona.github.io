---
layout:     post
title:      4.10函数闭包
subtitle:   JavaScript学习笔记
date:       2019-11-01
author:     gangyeona
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - JavaScript
---

## 作用域
作用域的好处是内部函数可以访问定义它们的外部函数的参数和变量（除了 this 和 arguments)

## 闭包
函数可以访问它被创建时所处的上下文环境，这被称为闭包。


```
var fade = function (node) {
    var level = 1;
    var step = function(node){
      var hex = level.toString(16)
      node.style.backgroundColor = '#FFFF' + hex + hex;
        if(level < 15){
          level += 1;
          setTimeout(step,100)
        }
    }
    setTimeout(step,100)
}
fade(document.body)

``` 
上文中`step`始终可以访问其上下文`level`

下面是经典的闭包案例：

```
/*
<div>
    <button>1</button>
    <button>2</button>
    <button>3</button>
    <button>4</button>
</div>
*/
var add_the_handlers = function (nodes) {
var i;
for(i = 0;i<nodes.length;i+=1){
   nodes[i].onclick = function (e){
     alert(i)
   }
}
}
add_the_handlers(document.getElementsByTagName("button"))
```
此时，无论当我们点击哪个按钮，弹出框的值总是3.
原因非常明显，当`add_the_handlers` 执行完成之后，`i`在上下文中的值已变成3,因此在`onclick`函数中的值也跟着变成了3。

一种解决方案是


```
  var add_the_handlers = function (nodes) {
    var i;
    for(i = 0; i < nodes.length; i+=1 ){
       nodes[i].onclick = function (e){
         return function(e){
          alert(i)
         }
       }(i)
    }
  }
```
此时，点击每个按钮弹出的就是其所处的序号值了。
可以简单理解成我们定义了一个函数并立即传递i去执行，此时我们的函数是`i`执行后的结果函数，而这个结果函数跟外层环境的`i`无关，其实就是它已经是`i`带入执行的结果了。
