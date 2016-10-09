---
title: doT模板简介
categories: 技术
toc: true
date: 2014-12-10 02:33:08
tags:
---

在多个公司的多个项目工作中，每一次我都极力推荐项目使用doT模板来减少工作量(避免字符串拼接)，提高工作效率。特别向大家推荐这个好东西来提高工作的幸福感。

## doT简介

doT是一个高性能JavaScript模板引擎，简单易用，其大小只有4KB（压缩后），适合各种数据和UI交互的地方使用，尤其适合MVC类WEB应用中，其也可以再NodeJS里面使用，并且表现出色。是本人觉得综合性能最好JavaScript模板引擎。

* 官方网站
http://olado.github.io/doT/index.html

* 下载地址
https://github.com/olado/doT

## 基本特点
* 不依赖：不依赖任何库（例如jQuery）和框架；
* 轻量：其大小只有4KB；
* 高效：渲染速度快，是JavaScript模板引擎中渲染速度最快几个之一；

## 基本用法
### HTML代码
	<!-- HTML模板部分（示例） -->
	<script id="simple-template" type="text/template">
	    {{ for (var i = 0, l = it.data.length; i < l; i++) { }}
	            <li class="widget-tab-item">
	                <a href="{{=it.data[i].link}}" class="site-link">
	                </a>
	            </li>
	    {{ } }}
	</script>

### JS代码
	//执行渲染语句：
	$("#simple-box").append(doT.template($("#simple-template").html())(simpleData));


## 渲染速度对比
### Chrome浏览器下
![chrome dot](/images/dot1.gif)

### Firefox浏览器下
![firefox dot](/images/dot2.gif)

### 综合对比
![dot](/images/dot3.png)


## 参考资料：
* [高效javascript模板引擎——doT.js原理及应用](http://www.cnf2e.com/javascript/dot-js.html)
* [doT.js 模板引擎的使用](http://www.fantxi.com/blog/archives/dot-template/)
* [各种JS模板引擎对比数据(高性能JavaScript模板引擎)](http://blog.csdn.net/wuchengzhi82/article/details/8938122)