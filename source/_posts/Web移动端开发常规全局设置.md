---
title: Web移动端开发常规全局设置
categories: 技术
toc: true
date: 2016-03-07 02:46:51
tags:
---

## 一，两种rem解决方案
### 1，根据dpi来设定rem以及viewport（美团的REM解决方案）
			//根据屏幕大小及dpi调整缩放和大小
	        (function() {
	            var scale = 1.0;
	            var ratio = 1;
	            if (window.devicePixelRatio >= 2) {
	                scale *= 0.5;
	                ratio *= 2;
	            }
	            var text = '<meta name="viewport" content="initial-scale=' + scale + ', maximum-scale=' + scale +', minimum-scale=' + scale + ', width=device-width, user-scalable=no" />';
	            document.write(text);
	            document.documentElement.style.fontSize = 50*ratio + "px";
	        })();

### 2，根据屏幕宽度来设置rem
		var autoChange=function(maxWidth,originSize){
		    var width=document.documentElement.clientWidth;
		    var Standard=originSize/(maxWidth*1.0/width);
		    Standard=Standard>100?100:Standard;
		    document.querySelector("html").style.fontSize=Standard+"px";
		};
		autoChange(750,100);

### 经测试第一种更佳。

## 二，移动端默认字体设置
 无衬线字体:

			body{
			    font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
			}


## 三，简单的flexbox布局

		/* 父级元素 */
		display: -webkit-box;
		display: -webkit-flex;
		display: flex;
		/* 子集元素 */
		-webkit-box-flex: 1;
		-webkit-flex: 1;
		flex: 1;


## 四，URL传递数据
		function request(paras) {
			    var url = location.href;
			    url = decodeURI(url);
			    var paraString = url.substring(url.indexOf("?") + 1, url.length).split("&");
			    var paraObj = {};
			    for (var i = 0; j = paraString[i]; i++) {
			        paraObj[j.substring(0, j.indexOf("=")).toLowerCase()] = j.substring(j.indexOf("=") + 1, j.length);
			    }
			    var returnValue = paraObj[paras.toLowerCase()];
			    if (typeof(returnValue) == "undefined") {
			        return "";
			    } else {
			        return returnValue;
			    }
		}

## 五、常规的清浮动

		  .clearfix:after {
		    visibility: hidden;
		    display: block;
		    font-size: 0;
		    content: " ";
		    clear: both;
		    height: 0;
		  }
		  .clearfix { display: inline-table; }
		    /* Hides from IE-mac \*/
		    * html .clearfix { height: 1%; }
		    .clearfix { display: block; }
		    /* End hide from IE-mac */

## 六、弹性滑动框架
### [iscroll5](http://cubiq.org/iscroll-5)
Scroll 5 is a faster more mature code than the previous versions. It doesn’t add many new features but it fixes bugs and most notably runs smoother on old devices. Please note that previous releases are not maintained nor supported, so go get the new version!

