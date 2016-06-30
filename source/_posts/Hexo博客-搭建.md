title: Hexo博客 - 搭建
date: 2015-11-25 16:40:52
tags: Hexo
---
## Hexo博客
<blockquote class="blockquote-center"> 偶然机会看到node.js博客，而只需要一个域名，太符合自己的要求，所以花了几天时间自己搭建了个，这里做个记录</blockquote>

### 前言
<!-- more -->
> 搭建**Hexo**博客需要2个准备，一个是git，一个是node.js。git这块不多讲，在另外篇章来详细讲述。



### git


github上建立仓库

登录后系统，在github首页，点击页面右下角「New Repository」

> 注：Github Pages的Repository名字是特定的，比如我Github账号是cnfeat，那么我Github Pages Repository名字就是cnfeat.github.io。

点击「Create Repository」 完成创建。


* 建立CNAME

接下来到github仓库，点击右下角的「Download ZIP」，下载源文件，解压，找到CNAME文件，用记事本打开，将cnfeat.com修改成你的域名，放进Hexo\source目录下，用hexo命令提交上去。
> 为之后的关联域名做准备

	$ hexo d -g
	
### node.js

* 下载[node.js](https://nodejs.org/en/)稳定版

* cd到指定的文件夹 安装Hexo


> -g 是全局 会加入到系统中 所有的文件都可以用

    $ npm install -g hexo
* 部署Hexo

有必要时 sudo 用起来..

	$ hexo init
	
Hexo随后会自动在目标文件夹建立网站所需要的所有文件。

	hexo generate （hexo g  也可以）
生成静态页面至hexo\\public\\目录。

* 本地启动

 启动本地服务，进行文章预览调试，命令：
 
 	$ hexo server (hexo s也可以)
 	
> 如果出现错误，运行一下命令, 生成本地服务

	$  npm install hexo-server --save
	
这个时候再执行：
	$ hexo s
	
得到:
 INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
 
 浏览器输入[http://localhost:4000](http://localhost:4000)  可以看到效果
 
 
### 关联域名

* 首先买域名，再解析到DNSPod
![tuian](http://ww4.sinaimg.cn/large/7a65bc01gw1eylq2f8lulj20yd0fe40o.jpg)
* 在DNSPod里面配置github的ip和io
 ![ndspod](http://ww4.sinaimg.cn/large/7a65bc01gw1eylq4hf1lqj20xx0h9q5z.jpg)
 ![pdo](http://ww4.sinaimg.cn/large/7a65bc01gw1eylq5oyrvhj20yd0h3acw.jpg)
 
 * 在你指定的文件夹里修改_config.yml
 ![url](http://ww1.sinaimg.cn/large/7a65bc01gw1eylq9me5w3j20gv0bgta9.jpg)
 
 接下来可以定位到你指定文件夹输入下面的代码查看效果
 
 	$ hexo g
 	$ hexo s
 在浏览器0.0.0.0可以看到
 
### 推送到远程
输入下面代码

	$ hexo d
如果没有效果，在指定的文件夹下面_config.yml修改
![config](http://ww3.sinaimg.cn/large/7a65bc01gw1eylqcskxlcj20gv0bgjt5.jpg)

再如果下面代码看效果

	$ hexo d -g

> 搭建博客总共也就这么多，以后会更新主题的一些细节
 