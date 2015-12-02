
title: iOS
type: index
date: 2015-11-25 21:53:27
---

### CoreAnimation动画

#### [CABasicAnimation](http://www.cnblogs.com/wengzilin/p/4250957.html)

在iOS中，图形可分为以下几个层次：

![tupian](http://images.cnitblog.com/blog/374539/201501/261752558947877.png)

越上层，封装程度越高，动画实现越简洁越简单，但是自由度越低；反之亦然。本文着重介绍Core Animation层的基本动画实现方案。
<!-- more -->

在iOS中，展示动画可以类比于显示生活中的“拍电影”。拍电影有三大要素：演员+剧本+开拍，概念类比如下：

> 演员--->CALayer，规定电影的主角是谁</br>
剧本--->CAAnimation，规定电影该怎么演，怎么走，怎么变换</br>
开拍--->AddAnimation，开始执行

### 一 、概念介绍

#### CALayer是什么呢？
  
CALayer是个与UIView很类似的概念，同样有layer,sublayer...，同样有backgroundColor、frame等相似的属性，我们可以将UIView看做一种特殊的CALayer，只不过UIView可以响应事件而已。一般来说，layer可以有两种用途，二者不互相冲突：一是对view相关属性的设置，包括圆角、阴影、边框等参数，更详细的参数请点击这里；```二是实现对view的动画操控。因此对一个view进行core animation动画，本质上是对该view的.layer进行动画操纵。```   

#### CAAnimation是什么呢？

CAAnimation可分为四种：

* CABasicAnimation

通过设定起始点，终点，时间，动画会沿着你这设定点进行移动。可以看做特殊的CAKeyFrameAnimation

* CAKeyframeAnimation

Keyframe顾名思义就是关键点的frame，你可以通过设定CALayer的始点、中间关键点、终点的frame，时间，动画会沿你设定的轨迹进行移动

* CAAnimationGroup

Group也就是组合的意思，就是把对这个Layer的所有动画都组合起来。PS：一个layer设定了很多动画，他们都会同时执行，如何按顺序执行我到时候再讲。

* CATransition

这个就是苹果帮开发者封装好的一些动画

#### 二、动手干活

实践出真知，看个例子就知道：

比如我们想实现一个类似心跳的缩放动画可以这么做，分为演员初始化、设定剧本、电影开拍三个步骤：

    - (void)initScaleLayer
    {
    //演员初始化
    CALayer *scaleLayer = [[CALayer alloc] init];
    scaleLayer.backgroundColor = [UIColor blueColor].CGColor;
    scaleLayer.frame = CGRectMake(60, 20 + kYOffset, 50, 50);
    scaleLayer.cornerRadius = 10;
    [self.view.layer addSublayer:scaleLayer];
    [scaleLayer release];
     
    //设定剧本
    CABasicAnimation *scaleAnimation = [CABasicAnimation animationWithKeyPath:@"transform.scale"];
    scaleAnimation.fromValue = [NSNumber numberWithFloat:1.0];
    scaleAnimation.toValue = [NSNumber numberWithFloat:1.5];
    scaleAnimation.autoreverses = YES;
    scaleAnimation.fillMode = kCAFillModeForwards;
    scaleAnimation.repeatCount = MAXFLOAT;
    scaleAnimation.duration = 0.8;
     
    //开演
    [scaleLayer addAnimation:scaleAnimation forKey:@"scaleAnimation"];
    }
 
 

    - (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
    [self initScaleLayer];
    }

效果请参考附图中的蓝色方块。其他效果可以依葫芦画瓢轻松实现。想要实现不同的效果，最关键的地方在于CABasicAnimation对象的初始化方式中keyPath的设定。在iOS中有以下几种不同的keyPath，代表着不同的效果：

![keypath](http://images.cnitblog.com/blog/374539/201501/270936547379029.png)

此外，我们还可以利用GroupAnimation实现多种动画的组合，在GroupAnimation中的各个动画类型是同时进行的。


    - (void)initGroupLayer
    {
    //演员初始化
    CALayer *groupLayer = [[CALayer alloc] init];
    groupLayer.frame = CGRectMake(60, 340+100 + kYOffset, 50, 50);
    groupLayer.cornerRadius = 10;
    groupLayer.backgroundColor = [[UIColor purpleColor] CGColor];
    [self.view.layer addSublayer:groupLayer];
    [groupLayer release];
   
    //设定剧本
    CABasicAnimation *scaleAnimation = [CABasicAnimation animationWithKeyPath:@"transform.scale"];
    scaleAnimation.fromValue = [NSNumber numberWithFloat:1.0];
    scaleAnimation.toValue = [NSNumber numberWithFloat:1.5];
    scaleAnimation.autoreverses = YES;
    scaleAnimation.repeatCount = MAXFLOAT;
    scaleAnimation.duration = 0.8;
     
    CABasicAnimation *moveAnimation = [CABasicAnimation animationWithKeyPath:@"position"];
    moveAnimation.fromValue = [NSValue valueWithCGPoint:groupLayer.position];
    moveAnimation.toValue = [NSValue valueWithCGPoint:CGPointMake(320 - 80,
                                                                  groupLayer.position.y)];
    moveAnimation.autoreverses = YES;
    moveAnimation.repeatCount = MAXFLOAT;
    moveAnimation.duration = 2;
     
    CABasicAnimation *rotateAnimation = [CABasicAnimation animationWithKeyPath:@"transform.rotation.x"];
    rotateAnimation.fromValue = [NSNumber numberWithFloat:0.0];
    rotateAnimation.toValue = [NSNumber numberWithFloat:6.0 * M_PI];
    rotateAnimation.autoreverses = YES;
    rotateAnimation.repeatCount = MAXFLOAT;
    rotateAnimation.duration = 2;
     
    CAAnimationGroup *groupAnnimation = [CAAnimationGroup animation];
    groupAnnimation.duration = 2;
    groupAnnimation.autoreverses = YES;
    groupAnnimation.animations = @[moveAnimation, scaleAnimation, rotateAnimation];
    groupAnnimation.repeatCount = MAXFLOAT;
    //开演
    [groupLayer addAnimation:groupAnnimation forKey:@"groupAnnimation"];
     }
    - (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
    [self initGroupLayer];
    }
    
 ![git](http://images.cnitblog.com/blog/374539/201501/281717520973529.gif)
 
 
 这种方式也是可以的。其实你的目的就是想对一个已经存在view进行CAAnimation控制，至于这个View是不是storyboard创建并不影响。在文章里layer都是新创建的，比如： 
 
     //演员初始化
     CALayer *scaleLayer = [[CALayer alloc]      init];
     scaleLayer.backgroundColor = [UIColor blueColor].CGColor;      
     scaleLayer.frame = CGRectMake(60, 100, 50, 50);
     scaleLayer.cornerRadius = 10;
     [self.view.layer addSublayer:scaleLayer];

如果你想控制已经创建出来的view，那就直接取出该View的layer，用下面的代码代替上面的创建过程：

     CALayer *scaleLayer = myExistingView.layer;

然后接下去就按照原来的方法处理