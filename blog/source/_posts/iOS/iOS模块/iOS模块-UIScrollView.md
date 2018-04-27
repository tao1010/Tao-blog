---
title: iOS模块-UIScrollView
date: 2018-04-13 16:41:00
tags: OC
categories: iOS

---

一、UIScrollView的基本知识		
1.属性		

```objectivec
_scrollView.bounces = NO;//不需要弹回
_scrollView.showsHorizontalScrollIndicator = NO;//不需要横向指示条
_scrollView.showsVerticalScrollIndicator = NO;//不需要纵向指示条
_scrollView.scrollEnabled = YES;//是否滚动
_scrollView.pagingEnabled = YES;//按页滚动
[_scrollView setContentOffset:CGPointMake(0, 0)];//不偏移
_scrollView.contentSize = CGSizeMake(100,200);//容器大小
```
2.代理方法		

```objectivec
//手指离开屏幕后ScrollView还会继续滚动一段时间只到停止
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView{
   NSLog(@"结束滚动后缓冲滚动彻底结束时调用");
}
 
-(void) scrollViewWillBeginDecelerating:(UIScrollView *)scrollView{
  NSLog(@"结束滚动后开始缓冲滚动时调用");
}
 
-(void)scrollViewDidScroll:(UIScrollView*)scrollView{
     //页面滚动时调用，设置当前页面的ID
    NSLog(@"视图滚动中X轴坐标%f",scrollView.contentOffset.x);
    NSLog(@"视图滚动中X轴坐标%f",scrollView.contentOffset.y);
}
 
-(void)scrollViewWillBeginDragging:(UIScrollView*)scrollView{
    NSLog(@"滚动视图开始滚动，它只调用一次");
}
 
-(void)scrollViewDidEndDragging:(UIScrollView*)scrollView willDecelerate:(BOOL)decelerate{
   NSLog(@"滚动视图结束滚动，它只调用一次");
}

```

二、导航栏渐变方法   
方法一：向上滑动或者向下滑动时，分别修改navigationBar的alpha值

```objectivec
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    
    self.currentPostion = scrollView.contentOffset.y;
    
    if (self.currentPostion > 0) {
        if (self.currentPostion - self.lastPosition >= 0) {
            if (self.topBool) {
                self.topBool = NO;
                self.bottomBool = YES;
                self.stopPosition = self.currentPostion + 64;
            }
            self.lastPosition = self.currentPostion;
            self.navigationController.navigationBar.alpha = 1 - self.currentPostion / 500;
        } else {
            if (self.bottomBool) {
                self.bottomBool = NO;
                self.topBool = YES;
                self.stopPosition = self.currentPostion + 64;
            }
            self.lastPosition = self.currentPostion;
            self.navigationController.navigationBar.alpha = (self.stopPosition - self.currentPostion) / 200;
        }
    }
}
解析：
	如果你不希望自己 navigationBar 上的按钮也被改变透明度，可以通过自定义 UINavigationController 实现。用系统的 navigationBar 时，navigationBar上的按钮的透明度也会被更改。
```
方法二：在navigationBar上加入一个view，修改view的alpha值

```objectivec
//View 的 userInteractionEnabled 属性要设置为 NO，不然会出现问题
- (void)configCustom{
	
	CGSize customSize = self.navigationController.navigationBar.frame.size;
	self.customView = [[UIView alloc] initWithFrame:CGRectMake(0, -20, customSize.width, customSize.height + 20)];
	self.customView.backgroundColor = [UIColor orangeColor];
	self.customView.userInteractionEnabled = NO;
	[self.navigationController.navigationBar insertSubview: self.customView atIndex:0];
}

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    
    self.currentPostion = scrollView.contentOffset.y;
    
    if (self.currentPostion > 0) {
        if (self.currentPostion - self.lastPosition >= 0) {
            if (self.topBool) {
                self.topBool = NO;
                self.bottomBool = YES;
                self.stopPosition = self.currentPostion + 64;
            }
            self.lastPosition = self.currentPostion;
            self.customView.alpha = 1 - self.currentPostion / 500;
        } else {
            if (self.bottomBool) {
                self.bottomBool = NO;
                self.topBool = YES;
                self.stopPosition = self.currentPostion + 64;
            }
            self.lastPosition = self.currentPostion;
            self.customView.alpha = (self.stopPosition - self.currentPostion) / 200;
        }
    }
}
```
方法三：自定义UINavigationControler类实现  

三、导航栏头像自由缩放   

```objectivec
@interface GDUserSettingController ()<UIScrollViewDelegate>

@property(nonatomic, strong) UIImageView    *headerImageView;

@end

- (void)initUserHeaderView{

    self.title = nil;
    UIView *titleView = [[UIView alloc] init];
    self.navigationItem.titleView = titleView;
    
    self.headerImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"phone_head"]];
    self.headerImageView.layer.cornerRadius = 35;
    self.headerImageView.layer.masksToBounds = YES;
    self.headerImageView.frame = CGRectMake(0, 0, 70, 70);
    // 这一句非常重要，保证用户头像水平居中
    self.headerImageView.center = CGPointMake(titleView.center.x, 70/4);
    [titleView addSubview:self.headerImageView];
}

//MARK:UIScrollViewDelegate
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    CGFloat offsetY = scrollView.contentOffset.y + scrollView.contentInset.top;
    
    CGFloat scale = 1.0;
    // 放大
    if (offsetY < 0) {
        // 允许下拉放大的最大距离为300
        // 1.5是放大的最大倍数，当达到最大时，大小为：1.5 * 70 = 105
        // 这个值可以自由调整
        scale = MIN(1.5, 1 - offsetY / 300);
    } else if (offsetY > 0) { // 缩小
        // 允许向上超过导航条缩小的最大距离为300
        // 为了防止缩小过度，给一个最小值为0.45，其中0.45 = 31.5 / 70.0，表示
        // 头像最小是31.5像素
        scale = MAX(0.45, 1 - offsetY / 300);
    }
    
    self.headerImageView.transform = CGAffineTransformMakeScale(scale, scale);
    
    // 保证缩放后y坐标不变
    CGRect frame = self.headerImageView.frame;
    frame.origin.y = -self.headerImageView.layer.cornerRadius / 2;
    self.headerImageView.frame = frame;
}
```



参考资料：	
