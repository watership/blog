---
title: 火狐扩展开发：在第三方页面插入JS脚本
categories: 技术
toc: true
date: 2014-05-23 02:15:59
tags:
---

## 第一步，首先在xul文件内引入执行插入脚本的JS文件，例如：
![js code](/images/20140523094924.png)

## 第二步，向第三方插入脚本的代码：
	var ffCreate = {
	    init: function () {
	        var appcontent = document.getElementById("appcontent"); // browser  详情见参考资料1
	        if (appcontent) {
	            appcontent.addEventListener("DOMContentLoaded", ffCreate.onPageLoad, false);//详情见参考资料2
	        }
	    },

	    onPageLoad: function (aEvent) {
	        var doc = aEvent.originalTarget;
	        var win = doc.defaultView;
	        ffCreate.injectScript(win, doc);
	    },

	    injectScript: function (win, doc) {
	        // insert the script to head
	        var daogw_s = doc.createElement('script');
	        daogw_s.charset = 'UTF-8';
	        daogw_s.type = 'text/javascript';
	        daogw_s.id = 'ffRemote';
	        daogw_s.src = '添加你要的地址';
	        doc.getElementsByTagName('head')[0].appendChild(daogw_s);
	    }
	};


	window.addEventListener("load", ffCreate.init, true);//等待第三方页面加载完成后，才把脚本添加到页面上

## 参考资料：
1. [MDN - Add-ons - On page load](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/On_page_load)
2. [事件DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/Reference/Events/DOMContentLoaded)
3. [MDN-XUL Overlays](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XUL/Overlays#Attaching_a_Script_to_an_Overlay)