---
title: 火狐扩展开发：普通Web页面和浏览器扩展互相通信
categories: 技术
toc: true
date: 2014-05-30 02:22:08
tags:
---

前篇博客介绍 [浏览器扩展在第三方页面引入JS脚本](http://tonychow.iteye.com/admin/blogs/2070222)的方法，但是插入在第三方页面里面的JS代码是无法调用浏览器扩展的API（如：[XPCOM Interface Reference](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XPCOM/Reference/Interface)），这样这些插入到第三方页面的代码就没办法和浏览器扩展进行通信，没办法交换数据，我们完全控制不了它们了。

对于这个问题Mozilla提供了一个方法：用DOM元素的属性来交互数据，这个方法非常妙；浏览器扩展可以获得第三方页面中任何DOM元素的属性，而这个页面的JS代码（就是我们插入的远程JS代码）显然也可以。
（官方文档地址：[Interaction between privileged and non-privileged pages](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/Interaction_between_privileged_and_non-privileged_pages)）

官方的文档写的比较罗嗦（[我试着翻译了一半没完成](https://developer.mozilla.org/zh-CN/docs/Code_snippets/Interaction_between_privileged_and_non-privileged_pages)），而我自己开发扩展只需要用到少量而且简单的数据的交换，所以我写一个转述。

## 页面向浏览器扩展发送数据
### 在页面HTML中创建一个DOM和自定义事件，代码如下：
	var element = document.createElement("MyExtensionDataElement");
	element.setAttribute("data-count", "foobar");
	document.documentElement.appendChild(element);

	var evt = document.createEvent("Events");
	evt.initEvent("MyExtensionEvent", true, false);
	element.dispatchEvent(evt);//触发事件
### 在扩展的browser.xul里面监听上面创建的自定义事件，代码如下：
	var MyExtension = {
	    myListener: function(evt) {
	        var count = evt.target.getAttribute("data-adcount");//这就是取到的数据
	        //alert(count);
	    }
	}

	document.addEventListener("MyExtensionEvent", function(e) { myExtension.myListener(e); }, false, true);
	// 最后一个值是火狐扩展的特权值，表示允许监听到普通页面的事件。

## 浏览器扩展向页面发送数据
### 在浏览器扩展的browser.xul中给页面创建一个自定义事件，触发事件可以改变页面中相应DOM元素的属性值，代码如下：
	var myExtension = {
	  myListener: function(evt) {
	    var count = evt.target.getAttribute("data-adcount");//这就是取到的数据
	    //alert(count);

	/* the extension answers the page*/
	    evt.target.setAttribute("attribute3", "The extension");

	    var doc = evt.target.ownerDocument;//取得页面的文档

	    var AnswerEvt = doc.createElement("MyExtensionAnswer");
	    AnswerEvt.setAttribute("Part1", "answers this.");

	    doc.documentElement.appendChild(AnswerEvt);

	    var event = doc.createEvent("HTMLEvents");
	    event.initEvent("MyAnswerEvent", true, false);
	    AnswerEvt.dispatchEvent(event);
	  }
	}

	document.addEventListener("MyExtensionEvent", function(e) { myExtension.myListener(e); }, false, true);

### 在页面中我们只要获取刚才设置的DOM元素的属性值就可以得到扩展内部发过来的数据了。
