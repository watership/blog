---
title: Mobile Web常见技术问题和规划
date: 2015-09-09 20:44:47
tags:
---

# Mobile Web常见技术问题和规划

前一段时间应Boss要求，给开发组同事做了一次关于前端技术的演讲，我整理了现在项目遇到的很多问题，和未来规划。我还特意去了一趟上海，求教朋友一些前端架构问题，他说我遇到的问题具有很广泛的普遍性，很多团队都有遇到，于是我抽空整理出来。

## 现状
### 一、目前前端技术
1. JS功能库：jQuery和Zepto混用；

2. 页面之间传递数据方法：url传递数据；

3. 本地数据存储cookie；

### 二、现存在的问题
1. 页面js以及css文件过多过大；

2. JS库和插件引用不规范不统一；

3. HTML片段管理和渲染方法需要改进和优化；

4. 没有进行JS模块化，通用方法未提出公用；

5. 线上JS和CSS代码未做压缩处理；

6. 没有数据缓存和更新机制；

7. 静态文件images需要规划管理；

![mobile phone](/images/mobile_web_dev_phone.jpg)


## 规划
### 一、统一JS使用规范和基本解决方案
1. 全局使用JS功能库Zepto
逐步将项目中每个页面的jQuery替换成Zepto，解决两者的兼容差异，和Zepto的功能缺失；

2. 全局引入移动点击事件处理方案Fastclick
解决click在移动设备上300ms延迟和点透bug；

3. 统一HTML渲染方法
从原有HTML字符串拼接逐步改为使用前端模板渲染引擎doT.js；

4. 数据缓存及更新机制
本地数据localStorage缓存一些ajax请求的数据，更新时间等机制写成公用的JS方法；

5. JavaScript模块化
CMD or AMD，计划引入seajs模块加载器；

### 二、布局和CSS开发
1. 公用样式文件的维护、CSS文件目录管理规范、单个页面CSS文件引入规范
	  * reset.css  (样式重置表)
	  * common.css  (公用样式表)
	  * page.css  (本页面样式)

2. 将JS动画效果逐步使用CSS3的动画进行取代，尽可能调用CSS3硬件加速

3. 后期有待选型和研究的问题
	  * 布局逐步放弃float布局，还是改用flex-box？
	  * REM or 百分比布局选型？
	  * LESS (SASS)是否适合引入?

### 三、前端代码可维护性
1. 命名规范
	  * 语义化命名
	  * CSS命名建议使用中杠来命名：help-guest-regist
	  * JS建议驼峰命名法：helpGuestRegist （方法命名尽可能的语义化和完整）

2. 结构html、样式css、行为js分离
	  * 避免HTML中写CSS样式
	  * 避免HTML中写JS语句

3. JavaScript全局变量
	  * 严格限制JS中全局变量的数量，避免定义过多全局变量；
	  * 注： [代码规范参考《编写可维护的JavaScript》](http://book.douban.com/subject/21792530/)
	![book](/images/mobile_web_dev_book1.jpg)

### 四、前端工作流改进
1. 多设备多浏览器自动同步刷新工具

[Browsersync](http://www.browsersync.cn/)
![server](/images/mobile_web_dev_server.jpg)

2. 前端代码上线前自动化处理
	  * JS和CSS代码的压缩；
	  * 给js/css/image文件版本号(文件指纹)：解决浏览器和微信强缓存问题；
	  * 将页面引入大量js和css文件合并成一个或多个文件（待解决）：减少http请求；
	  * 目前打算使用gulp工具实现发布前的文件处理的环节；

![gulp](/images/gulp_logo.jpg)

### 五、项目前端架构调整（长期规划）
#### SPA（Single-page application）
##### 优点：
1. 可以在页面切换间平滑过渡，增加Loading动画，类似native app；

2. 模块化开发；

3. 可以选择性的保留状态；

4. 避免了公共JS的反复执行，如无需在各个页面打开时都判断是否登录过等等；

5. 减少了请求体积，节省流量，加快页面响应速度；

##### 缺点：
 前期开发复杂度大大提升，工期过长，涉及方面过多，需要长期进行规划；

#### MVC：
计划采用Backbone.js
![backbone_logo](/images/backbone_logo.jpg)


### 资料：
[移动H5前端性能优化指南](http://isux.tencent.com/h5-performance.html)
