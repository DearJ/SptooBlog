title: iOS9网络Tip
date: 2016-03-02 14:25:09
tags:
- iOS
---
<blockquote class="blockquote-center">因为在ios9中请求网络变成了HTTPS模式了，所以新建工程的时候，需要改Plist文件，每次都要百度一下太麻烦，所以在这里记录一下，方便。</blockquote>


	<key>NSAppTransportSecurity</key>
	<dict>
    <!--Connect to anything (this is probably BAD)-->
    <key>NSAllowsArbitraryLoads</key>
    <true/>
	</dict>
	
> 打开plist文件的时候，右键选择Open As -> Source Code


![image](http://ww2.sinaimg.cn/large/7a65bc01jw1f1ii7okoaqj20zg0llq7v.jpg)