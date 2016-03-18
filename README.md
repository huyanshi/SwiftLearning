# SwiftLearning
swift学习之路，其中有自己总结的笔记和借鉴网络上的知识一起分享，如有重复请联系作者
##学习iOS开发，前期大多都是学习各种控件，但是在学习很长时候之后就发现控件基本都学的差不多，而且就会遇到一些问题。
### 今天我们学习一下

dispatch_async(dispatch_get_global_queue(0, 0)) { () -> Void in<br>
let imagefixed = self.clippingImage?.fixOrientation()<br>
while self.imageView == nil {<br>
            NSRunLoop.currentRunLoop().runMode(NSDefaultRunLoopMode, beforeDate: NSDate.distantFuture())<br>
        }<br>
            dispatch_sync(dispatch_get_main_queue(), { () -> Void in<br>
            self.imageView?.image = imagefixed<br>
            MBProgressHUD.hideHUDForView(self.view, animated: true)<br>
    })<br>

### 这段什么意思呢，有什么用呢，能做什么。我们围绕这个来说一下
* 1.如果imageView存在让RunLoop休眠也就是不执行，但是如果不存在那么就会执行这个线程
* 2.如果我们不通过这句实现这个作用的话，那么得在fixOrientation()这个方法里面添加判断这样看起来结构很乱
* 3.NSRunLoop的作用在于有事情做的时候使的当前NSRunLoop的线程工作，没有事情做让当前NSRunLoop的线程休眠

### 3月3日学习项目架构
* 架构的好处

### iOS开发中遇到的约束和frame，系统定义的控件有时候那约束改变不了大小 
### 苹果推荐斯坦福大学公开课，网易公开课可以看，以前的视频优酷下载挺不错哦
<br/>
### navigationController的navigationbar透明效果
####测试一 （效果有点过，title都透明了）

            func navigationController(navigationController: UINavigationController, didShowViewController viewController: UIViewController, animated: Bool) {
        if viewController == self {
            self.navigationController?.navigationBar.tintColor = UIColor(red: 0, green: 0, blue: 0, alpha: 1)
            self.navigationController?.navigationBar.alpha = 0.300
            self.navigationController?.navigationBar.translucent = true
        }else {
            self.navigationController?.navigationBar.alpha = 1
            // 背景颜色设置为系统默认颜色
            self.navigationController?.navigationBar.tintColor = nil
            self.navigationController?.navigationBar.translucent = false
        }
    }
####测试二 （没有起作用）

            在viewDidLoad中添加
            navigationController?.navigationBar.translucent = true
            navigationController?.navigationBar.shadowImage = UIImage()
            navigationController?.navigationBar.backgroundColor = UIColor.clearColor()
####测试三 （终于起作用）

            navigationController?.navigationBar.setBackgroundImage(UIImage(), forBarMetrics: UIBarMetrics.Compact)
            这句是关键性的一句，但是又会出现一个问题，就是navigationBar下面会出现一条线，也就是边框的线
