---
title: 浅谈JavaScript闭包(技术分享演讲稿)
categories: 技术
toc: true
date: 2016-08-03 02:57:06
tags:
---

# Javascript Closure

## 为什么知道闭包很重要？
闭包是JS语言的难点也是其一大特色，很多初学者甚至从业多年的开发者也很难弄清楚这个概念。弄明白这个概念十分重要，因为不但是对学习JS语言的一个大提升，在编写高级应用时候经常要用到，同时也是很多面试题目中经常会考到的问题。

## 闭包的“官方”解释：
所谓“闭包”，指的是一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。
想必上述解释大部分人看起来都像是天书，要了解闭包首先我们还需要知道JS语言的作用域这个概念。

## 变量的作用域
函数内部可以直接读取全局变量。

	var n=7;
	function f1 () {
		alert(n);
	}
	f1();  // 7

在函数外部自然无法读取函数内的局部变量。

		function f1 () {
		  var n=7;
		}
		alert(n);  // error

这样牵扯出来一个问题：如何访问函数内部的变量呢 ？
我们先来看下面一个例子：

		function f1() {
		   var n = 7;
		   function f2 (){
		      alert(n);
		   }
		   return f2;
		}
		var result = f1();
		result();
这里就形成了闭包，由此我们可以给闭包一个新的解释。

## 闭包的“通俗”解释：
闭包就是能够读取其他函数内部变量的函数，其本质就是就是将函数内部和函数外部连接起来的一座桥梁。

（闭包是由函式和与其相关的参照环境组合而成的实体。即便在函数内部定义了函数，如果没有引用父函数作用域的变量，也照样可以不是闭包。函数和环境，缺一不可，这才是闭包。）

再看一个稍微复杂一点的例子：

		var foo = ( function () {
		    var secret = 'secret';
		    return {
		        get_secret: function () {
		            return secret;
		        },
		new_secret: function (new_secret) {
		            secret = new_secret;
		        }
		    };
		}() );
		foo.get_secret ();  // 得到 ‘secret'
		foo.secret;   // Type error，访问不能
		foo.new_secret ('a new secret');  // 通过函数接口，我们访问并修改了 secret 变量
		foo.get_secret (); // 得到 'a new secret'

>引用 Douglas Crockford [1] :
>之所以可能通过这种方式在 JavaScript 种实现公有，私有，特权变量正是因为闭包，闭包是指在 JavaScript 中，内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后。

### 闭包主要应用场合主要是为了：设计私有的方法和变量。

		function foo(x) {
		    var tmp = 3;
		    return function (y) {
		        alert(x + y + (++tmp));
		    }
		}
		var bar = foo(2); // bar 现在是一个闭包
		bar(10);

## 闭包特点
闭包会把函数中的变量都被保存在内存中。
### 注意点
1. 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除
2. 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

## 参考资源
* [ECMA-262-3 in detail. Chapter 6. Closures.](http://dmitrysoshnikov.com/ecmascript/chapter-6-closures/)

* [how do JavaScript closures work](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work)

* [学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)

* [理解Javascript的闭包](http://coolshell.cn/articles/6731.html)

* [JavaScript：闭包和引用](http://bonsaiden.github.io/JavaScript-Garden/zh/#function.closures)

* [Private Members in JavaScript](http://www.crockford.com/javascript/private.html)


该篇文章是公司技术分享会的演讲稿文字整理,附件可点击[下载](http://7i7gok.com1.z0.glb.clouddn.com/javascript_closure_ppt.pdf)。
