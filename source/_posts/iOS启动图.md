title: iOS启动图
date: 2015-12-03 14:29:32
tags: 
 - iOS

categories: iOS - skill

---

<blockquote class="blockquote-center">启动图是每个应用程序都需要的，所以在这里打个mark。</blockquote>
### 前言
<!-- more -->

在设置启动图前，你首先要准备好你得启动图，并且裁剪好相应地尺寸，对应如下：

![image](http://ww3.sinaimg.cn/large/7a65bc01gw1eymgqmstzqj20fk0773zi.jpg)

> 图片的命名也要有一定的规则，一定要以Default开头,如果需要，后面跟-xxxh,这个xxx跟你得尺寸一致。如下图：

![default](http://ww2.sinaimg.cn/large/7a65bc01gw1eymh0wiflbj20bb0aa0tj.jpg)

### xcode设置
* 首先在Images.xcassets里面新建LaunchImage

![launchImage](http://ww2.sinaimg.cn/large/7a65bc01gw1eynhs9ljtbj20pj0k7q79.jpg)

* 然后在后面的项目栏里选中第三个（向下的箭头）

> landscape是横向，portrait是纵向 


因为没有ipad，所以把ipad的都给勾去，最后只保留3个就可以了，对应的都是iPhone：Portrait。

然后把对应的图片拖入中间的框就行了，对应如下

|       1x    |       2x    |   Retina 4  |Retina HD 5.5|Retina HD 4.7|
|-------------|-------------|-------------|-------------|-------------|
|   320x480   |   640x960   |   640x1136  |  1242x2208  |   750x1334  |
| Default.png |Default@2x.png|Default-568h@2x.png|Default-2208h.png|Default-1334h.png|

> 因为要拖入7张图片，可是有2张是重复的，2x和Retina 4。所以Show in Finder,找到对应的文件夹，发现多出现了2张图，是-1，把他们删除就可以，文件夹内还有Contents.json。打开后把里面对应的-1 2个字母删掉就可以，用得还是同一张图。

* 最后在General里面配置一下

![general](http://ww3.sinaimg.cn/large/7a65bc01gw1eymhnzydnrj20ia03tdg6.jpg)

> ok 结束了  看效果吧



