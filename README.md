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
PS：设置导航title颜色[bar setTitleTextAttributes:@{ NSForegroundColorAttributeName :[UIColor whiteColor]}];
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
            寻找一下边框线也是这个枚举就可以解决
            navigationController?.navigationBar.setBackgroundImage(UIImage(), forBarMetrics: UIBarMetrics.Default)
            完美解决，获取有更好的办法，找到之后更新
#####问题又出现了，当前控制器设置过的navigationBar，在push控制器的时候页面会显示不正常会向下偏移64个点，并且导航栏会变黑
ps: 导航栏变黑是因为没有给navigationBar配置颜色，在上一个控制器已经设置为半透明，也就是translucent这个属性，navigationBar底部的线条，shadowImage设置就没有了，self.extendedLayoutIncludesOpaqueBars = true这个属性会另显示不正确具体原因还要在找。
#####总结一下，设置透明只需要设置navigationBar.setBackgroundImage的BarMetrics.Default和给一个shadowImage就没有线条，还需要一个属性translucent = true 就可以了
###iOS开发中设置navigationBar设置中遇见的一系列问题
    * 在页面设置透明时候跳转之后，其他的页面也会透明
    * 在viewWilldisAppear之后其他的页面导航有的变黑有的页面正常
    * 在设置之后页面的显示会不正常，向下偏移
    * 在使用非系统输入法APP崩溃……
    * 其他的问题持续发生，一个问题涉及面比较广，产生问题的页面比较多，要看看发生问题的原因解决
###在实际应用中建议使用KVO对navigationBar设置这样设置会好处理一些，自定义导航栏，在viewwillappera和viewwilldidappear设置这样不影响其他页面

