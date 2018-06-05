---
title: iOS问题-控件
date: 2018-06-05 15:53:12
tags: 问题
categories: iOS
---

一、UIButton		
1.实现按钮倒计时文本闪烁		
	
	一般和NSTimer结合使用容易出现;
	
	解决方法：
	UIButtonTypeSystem --> UIButtonTypeCustom
2.图文排序		
UIEdgeInsets insets = {top, left, bottom, right}    
![edgeinsets](edgeinsets.png)	     

	蓝色标识为可变区域， 绿色标识为不变区域。
	UIEdgeInsets结构体的属性top与bottom为一对，用来指定纵向可变区域（黑色虚线矩形），left与right为一对，用来指定横向可变区域(白色虚线矩形)。
	当UIButton/UIImageView的size大于UIImage的size时，会调整图片中可变区域大小以铺满整个控件,具体调整规则如下：
		(1)控件宽度大于图片宽度，拉伸白色虚线矩形
		(2)控件高度大于图片高度，拉伸黑色虚线矩形
		(3)控制宽度小于图片宽度时，横向整体缩小(可变区与不变区比例不变)
		(4)控制高度小于图片高度时，纵向整体缩小(可变区与不变区比例不变)


	
	
	
	
二、UITableView		
1.最后一个Cell不显示分割线		

	解决方法：

	//Objective-C
	- (void)layoutSubviews {
	    [super layoutSubviews];
	    for (UIView *subview in self.contentView.superview.subviews) {
	        if ([NSStringFromClass(subview.class) hasSuffix:@"SeparatorView"]) {
	            subview.hidden = NO;
	            CGRect frame = subview.frame;
	            frame.origin.x += self.separatorInset.left;
	            frame.size.width -= self.separatorInset.right;
	            subview.frame =frame;
	        }
	    }
	}
	//Swift
	override func layoutSubviews() {
	
	    super.layoutSubviews()
	    for item in self.contentView.superview!.subviews {
	        var subview = item as! UIView
	        if NSStringFromClass(subview.classForCoder).hasSuffix("SeparatorView") {
	            subview.hidden = false
	            var frame = subview.frame
	            frame.origin.x += self.separatorInset.left
	            frame.size.width -= self.separatorInset.right
	            subview.frame  = frame
	        }
	    }
	}

三、UINavigationBar		
1.导航栏渐变	
	
	方法一:
	思路: 
		向下、上滑动时分别对navigationBar的alpha进行更新;
	tips:
		如果你不希望自己 navigationBar 上的按钮也被改变透明度，可以通过自定义 UINavigationController 实现。
		用系统的 navigationBar 时，navigationBar上的按钮的透明度也会被更改;
	eg:
		#pragma mark - UIScrollViewDelegate
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
	方法二:
	思路:
		在 navigationBar 上加一个 view，然后设置 view 的 alpha;
	eg:
		#pragma mark - UIScrollViewDelegate
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
	
	方法三：
	思路：
		自定义导航控制器实现

四、UIImageView		
1.动画实现		
	
	UIImageView *imageView = [UIImageView alloc] init];
	//设置动画帧
	imageView.animationImages=[NSArray arrayWithObjects:
		 [UIImage imageNamed:@"gc_service_0"],
		 [UIImage imageNamed:@"gc_service_1"],
	     [UIImage imageNamed:@"gc_service_2"],
	     [UIImage imageNamed:@"gc_service_3"],
	     [UIImage imageNamed:@"gc_service_4"],
	     [UIImage imageNamed:@"gc_service_5"],nil ];
	imageView.animationDuration = 1.0//设置动画总时间
	imageView.animationRepeatCount = 0;//设置重复次数,0表示不重复
	[imageView startAnimating];   //开始动画
五、指定位置圆角		
以UIButton为例:
	
	UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
	
	按钮 底部 - 左边 - 圆角
	UIRectCorner corners =  UIRectCornerBottomLeft;
	按钮 底部 - 左边+右边 - 圆角
	UIRectCorner corners =  UIRectCornerBottomLeft | UIRectCornerBottomRight;
	按钮  底部(左) + 顶(右) - 圆角
	UIRectCorner corners =  UIRectCornerBottomLeft | UIRectCornerTopRight;
	...	
	
	UIBezierPath *maskPath = [UIBezierPath bezierPathWithRoundedRect:button.bounds byRoundingCorners:corners cornerRadii:CGSizeMake(10, 10)];

    CAShapeLayer *maskLayer = [[CAShapeLayer alloc] init];
    maskLayer.frame = button.bounds;
    maskLayer.path = maskPath.CGPath;
    button.layer.mask = maskLayer;
	
	枚举：
	typedef NS_OPTIONS(NSUInteger, UIRectCorner) {
	    UIRectCornerTopLeft     = 1 << 0,
	    UIRectCornerTopRight    = 1 << 1,
	    UIRectCornerBottomLeft  = 1 << 2,
	    UIRectCornerBottomRight = 1 << 3,
	    UIRectCornerAllCorners  = ~0UL
	};
	
	

参考资料:	
1.[有footerView时最后一个cell不显示分割线](http://www.cnblogs.com/lancely/p/5782784.html)	    
2.[导航栏渐变](http://blog.cocoachina.com/article/57383) 	
3.[UIButton的图文排序](https://www.jianshu.com/p/3052a3b14a4e)		 
4.[UIEdgeInsetsMake-重要](https://www.jianshu.com/p/0d3dbc30fad5)     
