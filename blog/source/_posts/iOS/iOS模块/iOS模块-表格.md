---
title: iOS模块-表格
date: 2018-03-25 15:20:28
tags: iOS模块
categories: iOS
---

一、业务需求  
![PM](pm.jpg)

1.左右滑动 -  表格第一列不动;   
2.上下滑动 - 表格第一行不动;   
3.为了可扩展性,考虑可点击;   
	
一、实现思路   
![示意图](table.png)

1.组成：
	
	左边淡黄色A - UITableView 构成表格的第一列;
	右边绿色B - UIScrollView 
		橙红色C- UITableView构成表格右边可滑动区域;
2.实现：
	
	1.充分运用UITableView的HeaderView的悬停效果，设置 A、C的属性style:UITableViewStylePlain；
	2.为了左右滑动，结合第一行标题数量，设置UIScrollView的contentSize；
	3.将C - UITableView加载到B上；
	4.数据模型处理，若可点击可用UIButton作为文本的显示容器,对应的数据模型Id赋值给UIButton的tag；
	5.关联滑动，处理事件；
二、关联滑动     

``` objectiveC
UIScrollView的delegate方法：
-(void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    if (A - UITableView) {//第一列
        //滑动第一列 关联右边
        CGFloat offsetY = A.contentOffset.y;
        CGPoint timeOffsetY = C.contentOffset;
        timeOffsetY.y = offsetY;
        C.contentOffset = timeOffsetY;
        
        if(offsetY == 0){
            
            C.contentOffset = CGPointZero;
        }
        
    }else{
        C - UITableView
        //滑动右边 管理左边
        CGFloat offsetY = C.contentOffset.y;
        
        CGPoint timeOffsetY = A.contentOffset;
        timeOffsetY.y = offsetY;
        A.contentOffset = timeOffsetY;
        
        if(offsetY == 0){
            
            A.contentOffset = CGPointZero;
        }
    }
}

```
