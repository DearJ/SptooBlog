title: cocoapods最新教程
date: 2015-12-04 10:06:53
tags:
 - iOS

categories: iOS - skill

---
<blockquote class="blockquote-center">cocoapods是一个管理第三方库的插件，能很好帮助你管理第三方库，而也给了你组件化模块提供了可能，因此cocoapod你必须要掌握好。</blockquote>

### Get on with building your app, not duplicating code.

<!-- more -->
> 因为唐巧有写过[cocoapods](http://blog.devtang.com/blog/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/)的文章，所以这里借鉴一下，而且在此基础上更新一下
### CocoaPods 简介
每种语言发展到一个阶段，就会出现相应的依赖管理工具，例如 Java 语言的 Maven，nodejs 的 npm。随着 iOS 开发者的增多，业界也出现了为 iOS 程序提供依赖管理的工具，它的名字叫做：CocoaPods。

CocoaPods项目的源码 在** Github **上管理。该项目开始于 2011 年 8 月 12 日，经过多年发展，现在已经成为 iOS 开发事实上的依赖管理标准工具。开发 iOS 项目不可避免地要使用第三方开源库，CocoaPods 的出现使得我们可以节省设置和更新第三方开源库的时间。

我在开发猿题库客户端时，使用了 24 个第三方开源库。在没有使用** CocoaPods **以前，我需要:

1. 把这些第三方开源库的源代码文件复制到项目中，或者设置成**git**的**submodule**。
2. 对于这些开源库通常需要依赖系统的一些 framework，我需要手工地将这些 framework 分别增加到项目依赖中，比如通常情况下，一个网络库就需要增加以下 framework: CFNetwork, SystemConfiguration, MobileCoreServices, CoreGraphics, zlib。
3. 对于某些开源库，我还需要设置```$ -licucore```或者 ```$ -fno-objc-arc```等编译参数
4. 管理这些依赖包的更新。

这些体力活虽然简单，但毫无技术含量并且浪费时间。在使用 CocoaPods 之后，我只需要将用到的第三方开源库放到一个名为 Podfile 的文件中，然后```$ pod install。 ```CocoaPods 就会自动将这些第三方开源库的源码下载下来，并且为我的工程设置好相应的系统依赖和编译参数。

### CocoaPods 的安装和使用介绍

#### 安装
安装方式异常简单 , Mac 下都自带 ruby，使用 ruby 的 gem 命令即可下载安装：

	$ sudo gem install cocoapods
	$ pod setup
	
> 安装和更新pod，代码都是一样的，不过我更新过10.10的系统，上面那个代码不行了，要改成下面的代码才行。

	$ sudo gem install -n /usr/local/bin cocoapods

如果你的 gem 太老，可能也会有问题，可以尝试用如下命令升级 gem:

	$ sudo gem update --system

另外，ruby 的软件源** https://rubygems.org **因为使用的是亚马逊的云服务，所以被墙了，需要更新一下 ruby 的源，使用如下代码将官方的 ruby 源替换成国内淘宝的源：

	$ gem sources --remove https://rubygems.org/
	$ gem sources -a http://ruby.taobao.org/
	$ gem sources -l
> 注意：因为现在淘宝镜像不能用了，在网上查了下原因，原来是要更新成https。

	$ gem sources -a https://ruby.taobao.org/
	
还有一点需要注意，```pod setup```在执行时，会输出```Setting up CocoaPods master repo```，但是会等待比较久的时间。这步其实是** Cocoapods **在将它的信息下载到``` ~/.cocoapods```目录下，如果你等太久，可以试着 cd 到那个目录，用```du -sh *```来查看下载进度。你也可以参考本文接下来的使用``` cocoapods 的镜像索引```一节的内容来提高下载速度。

#### 查看pod版本
	$ pod --version
### 使用 CocoaPods 的镜像索引
所有的项目的** Podspec **文件都托管在```https://github.com/CocoaPods/Specs```。第一次执行```pod setup```时, **CocoaPods** 会将这些 *podspec* 索引文件更新到本地的``` ~/.cocoapods/```目录下，这个索引文件比较大，有 80M 左右。所以第一次更新时非常慢，笔者就更新了将近 1 个小时才完成。

一个叫**akinliu**的朋友在 **gitcafe** 和 **oschina** 上建立了 CocoaPods 索引库的镜像，因为 gitcafe 和 oschina 都是国内的服务器，所以在执行索引更新操作时，会快很多。如下操作可以将 CocoaPods 设置成使用 gitcafe 镜像：

	$ pod repo remove master
	$ pod repo add master https://gitcafe.com/akuandev/Specs.git
	$ pod repo update
	
将以上代码中的```https://gitcafe.com/akuandev/Specs.git``` 替换成 ```http://git.oschina.net/akuandev/Specs.git``` 即可使用 oschina 上的镜像。

### 使用 CocoaPods

使用时需要新建一个名为 Podfile 的文件，以如下格式，将依赖的库名字依次列在文件中即可

在终端中进入（cd命令）你项目所在目录，然后在当前目录下，利用vim创建Podfile，运行：

	$ vim Podfile
	
然后在Podfile文件中先输入**i**进入编辑模式，再输入以下文字：

	$ platform :ios, '7.0'
	$ pod 'JSONKit',       '~> 1.4'
	$ pod 'Reachability',  '~> 3.0.0'
	$ pod 'RegexKitLite'
	$ pod "AFNetworking", "~> 2.0"
	
注意，这段文字不是小编凭空生成的，可以在AFNetworking的github页面找到。这两句文字的意思是，当前AFNetworking支持的iOS最高版本是iOS 7.0, 要下载的AFNetworking版本是2.0。

然后保存退出。vim环境下，保存退出命令是：

	$ :wq

这时候，你会发现你的项目目录中，出现一个名字为Podfile的文件，而且文件内容就是你刚刚输入的内容。注意，Podfile文件应该和你的工程文件.xcodeproj在同一个目录下。

这时候，你就可以利用CocoPods下载AFNetworking类库了。还是在终端中的当前项目目录下，运行以下命令：

	$ cd "your project home"
	$ pod install

现在，你的所有第三方库都已经下载完成并且设置好了编译参数和依赖，你只需要记住如下 2 点即可：

1. 使用 CocoaPods 生成的 .xcworkspace 文件来打开工程，而不是以前的 .xcodeproj 文件。
2. 每次更改了** Podfile **文件，你需要重新执行一次```pod update```命令。

### 查找第三方库
你如果不知道 cocoaPods 管理的库中，是否有你想要的库，那么你可以通过 pod search 命令进行查找，以下是我用 pod search json 查找到的所有可用的库：

	$ pod search json

    -> AnyJSON (0.0.1)
    Encode / Decode JSON by any means possible.
    - Homepage: https://github.com/mattt/AnyJSON
    - Source:   https://github.com/mattt/AnyJSON.git
    - Versions: 0.0.1 [master repo]


    -> JSONKit (1.5pre)
    A Very High Performance Objective-C JSON Library.
    - Homepage: https://github.com/johnezang/JSONKit
    - Source:   git://github.com/johnezang/JSONKit.git
    - Versions: 1.5pre, 1.4 [master repo]

    // ... 以下省略若干行

### 关于 Podfile.lock

当你执行```pod install```之后，除了 Podfile 外，CocoaPods 还会生成一个名为**Podfile.lock**的文件，Podfile.lock 应该加入到版本控制里面，不应该把这个文件加入到.gitignore中。因为**Podfile.lock**会锁定当前各依赖库的版本，之后如果多次执行```pod install``` 不会更改版本，要pod update才会改**Podfile.lock**了。这样多人协作的时候，可以防止第三方库升级时造成大家各自的第三方库版本不一致。

CocoaPods 的这篇 官方文档 也在What is a Podfile.lock一节中介绍了Podfile.lock的作用，并且指出：
> his file should always be kept under version control.

### 为自己的项目创建 podspec 文件

我们可以为自己的开源项目创建**podspec**文件，首先通过如下命令初始化一个**podspec**文件：
	
	$ pod spec create your_pod_spec_name
该命令执行之后，CocoaPods 会生成一个名为```your_pod_spec_name.podspec```的文件，然后我们修改其中的相关内容即可。

具体步骤可以参考这两篇博文中的相关内容：

* [《如何编写一个CocoaPods的spec文件》](http://ishalou.com/blog/2012/10/16/how-to-create-a-cocoapods-spec-file/)
* [《Cocoapods 入门》](http://studentdeng.github.io/blog/2013/09/13/cocoapods-tutorial/)

### 使用私有的 pods
我们可以直接指定某一个依赖的**podspec**，这样就可以使用公司内部的私有库。该方案有利于使企业内部的公共项目支持 CocoaPods。如下是一个示例：

	$ pod 'MyCommon', :podspec => 'https://yuantiku.com/common/myCommon.podspec'

### 不更新 podspec
CocoaPods 在执行```pod install```和```pod update```时，会默认先更新一次podspec索引。使用```--no-repo-update```参数可以禁止其做索引更新操作。如下所示：

	$ pod install --no-repo-update
	$ pod update --no-repo-update

### 生成第三方库的帮助文档

如果你想让 CococaPods 帮你生成第三方库的帮助文档，并集成到 Xcode 中，那么用 brew 安装 appledoc 即可：

#### 安装Homebrew
打开终端窗口, 粘贴以下脚本：

	$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

安装 appledoc

	$ brew install appledoc
	
关于 appledoc，我在另一篇博客 [《使用 Objective-C 的文档生成工具:appledoc》](http://blog.devtang.com/blog/2012/02/01/use-appledoc-to-generate-xcode-doc/) 中有专门介绍。它最大的优点是可以将帮助文档集成到 Xcode 中，这样你在敲代码的时候，按住 opt 键单击类名或方法名，就可以显示出相应的帮助文档。

### 原理

大概研究了一下 CocoaPods 的原理，它是将所有的依赖库都放到另一个名为 Pods 项目中，然后让主项目依赖 Pods 项目，这样，源码管理工作都从主项目移到了 Pods 项目中。发现的一些技术细节有：

1. Pods 项目最终会编译成一个名为 libPods.a 的文件，主项目只需要依赖这个 .a 文件即可。
2. 对于资源文件，CocoaPods 提供了一个名为 Pods-resources.sh 的 bash 脚本，该脚本在每次项目编译的时候都会执行，将第三方库的各种资源文件复制到目标目录中。
3. CocoaPods 通过一个名为 Pods.xcconfig 的文件来在编译时设置所有的依赖和参数。
