# preferredStatusBarStyle-
最近在处理项目中警告⚠️问题
因为项目中导航栏颜色有很多种从而状态栏就会产生二种 
在 iOS 2.0 --> 9.0
###UIStatusBarStyleDefault
###UIStatusBarStyleLightContent
```
//之前项目设置状态栏都是这种,但这种在iOS9.0后就被遗弃
 [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleDefault];//'setStatusBarStyle:' is deprecated: first deprecated in iOS 9.0 - Use -[UIViewController preferredStatusBarStyle]
 [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
```
因为在iOS9.0就被遗弃了,这些方法会报警告,所以强迫症不用了,根据他的提示用
###preferredStatusBarStyle
```
@property(nonatomic, readonly) UIStatusBarStyle preferredStatusBarStyle NS_AVAILABLE_IOS(7_0) __TVOS_PROHIBITED; // Defaults to UIStatusBarStyleDefault 
```
而且最低支持 iOS 7，用户手机系统适配方面没问题.
开始着手用在ViewController 里面用preferredStatusBarStyle
```
- (UIStatusBarStyle)preferredStatusBarStyle{
    NSLog(@"go style");
    return UIStatusBarStyleLightContent;
}
```
发现根本状态栏的颜色没变,然后打印也没有走,一脸懵逼.
然后重新创建一个项目,直接写这个- (UIStatusBarStyle)preferredStatusBarStyle方法发现走了,颜色也变了,再次懵逼了.

是不是跟  setStatusBarStyle 有冲突,于是回忆setStatusBarStyle需要注意的东西,想起之前设置 状态栏颜色 在info.plist 中 View controller-based status bar appearance 设置为NO 才能改变状态栏的颜色,把 View controller-based status bar appearance 设置为YES 再跑一变发现走了preferredStatusBarStyle 颜色也变了.

### 在NavigationController 中写
```
//当设置了 childViewControllerForStatusBarStyle 后，不会进入这里
- (UIStatusBarStyle)preferredStatusBarStyle {
    return UIStatusBarStyleDefault;
}

//以 topViewController 的 preferredStatusBarStyle 来设置 statusBarStyle
- (UIViewController *)childViewControllerForStatusBarStyle {
    return self.topViewController;
}
```
###当你想改变状态栏颜色时在ViewController
```
- (UIStatusBarStyle)preferredStatusBarStyle{
    return UIStatusBarStyleLightContent;
}
```
#如果之前在info.plist 中 View controller-based status bar appearance 设置 NO,一定要改为为 YES,如果之前没有设置就可以不用管,不然preferredStatusBarStyle 不调用
