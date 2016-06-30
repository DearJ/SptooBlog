title: iOS开发中集成Reveal
date: 2016-03-05 16:36:53
tags:
- Tool
---
<blockquote class="blockquote-center">Reveal是iOS中很好的调试工具，可以看视图的层级，也可以实时修改其中视图的参数，更可以破解好的App的视图层级。</blockquote>
### 前言

在网上看到很多人推荐这款工具，所以自己也就研究了下，在这记录一下安装的过程。
<!-- more -->

### iOS开发中集成Reveal
[Reveal](http://revealapp.com/)是一个界面调试工具。使用Reveal，我们可以在iOS开发时动态地查看和修改应用程序的界面。它类似Chrome的“审查元素”功能，我们不但可以在运行时看到iOS程序的界面层级关系，还可以实时地修改程序界面，不用重新运行程序就可以看到修改之后的效果。

在使用时，我们将Reveal连接上模拟器或真机上正在运行的iOS程序，然后就可以查看和调试iOS程序的界面。

### 集成Reveal有2种方法：

#### 静态连接
> 注意：这种方式会修改工程，网上教程很多，这里就不多介绍了

#### 不修改您的Xcode工程并加载Reveal
> 注意：此方法仅适用于在iOS模拟器上运行的应用。

   1.打开您的iOS工程，选择 View → Navigators → Show Breakpoint Navigator。

   2.在面板左下角，点击 + 按钮并选择**Add Symbolic Breakpoint**。
   
   3.在 Symbol 输入区内输入`UIApplicationMain`。
   
   4.点击 Add Action 按钮, 确认 Action 被设置为 Debugger Command。
   
   5.将以下内容拷贝到 Action 的输入区内:

		expr (Class)NSClassFromString(@"IBARevealLoader") == nil ? (void *)dlopen("/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib", 0x2) : ((void*)0)
		
> 注意: 请确认Reveal.app的路径信息符合您Mac的实际位置。

   6.选中 Automatically continue after evaluating actions 选项。
   
   7.右击刚才新创建的断点，选择 Move Breakpoint To → User.
   
   8.在iOS模拟器上构建并运行您的应用。
 
如果一切正常运行，请切换到Reveal应用，此时您的应用应会出现在应用选择器的下拉列表当中。选中您的应用，确认可以看到此时正在模拟器中运行的应用界面截图。

在使用时，我们将Reveal连接上模拟器或真机上正在运行的iOS程序，然后就可以查看和调试iOS程序的界面。
 