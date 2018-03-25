---
title: iOS模块-UITableView
date: 2018-03-25 15:59:51
tags: iOS
---

一、补齐Cell的分割线

只有左右间距可用,如果想将两边都补齐则设置为UIEdgeInsetsMake(0, 0, 0, 0)即可;

```objectivec
-(void)viewDidLayoutSubviews {
    if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {
        [self.tableView setSeparatorInset:UIEdgeInsetsMake(0, 0, 0, 0)];
    }
    if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)])  {
        [self.tableView setLayoutMargins:UIEdgeInsetsMake(0, 0, 0, 0)];
    }
}

-(void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath*)indexPath{
    if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        [cell setLayoutMargins:UIEdgeInsetsMake(0, 0, 0, 0)];
    }
    if ([cell respondsToSelector:@selector(setSeparatorInset:)]){
        [cell setSeparatorInset:UIEdgeInsetsMake(0, 0, 0, 0)];
    }
}

```
ps：
	
	UIEdgeInsetsMake(CGFloat top, CGFloat left, CGFloat bottom, CGFloat right)

二、修改Cell的默认显示图片（UITableViewCellAccessoryDisclosureIndicator）

``` objectivec
 UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:identifier];
	if(cell == nil){
	    
	    cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:identifier];
	    UIImageView *accessoryView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, 7.5, 13)];
	    accessoryView.image = [UIImage imageNamed:@"jxwc_right_more"];
	    cell.accessoryView = accessoryView;
	    cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
	}
```
三、Cell在点击状态下，子控件不变色

重写UITableViewCell的方法：

``` objectivec
- (void)setSelected:(BOOL)selected animated:(BOOL)animated;
- (void)setHighlighted:(BOOL)highlighted  animated:(BOOL)animated;
```

实现：   
OC:

```objectivec
- (void)setSelected:(BOOL)selected animated:(BOOL)animated{

    // 获取 contentView 所有子控件
    NSArray<__kindof UIView *> *subViews = self.contentView.subviews;
    // 创建颜色数组
    NSMutableArray *colors = [NSMutableArray array];
    
    for (UIView *view in subViews) {
        // 获取所有子控件颜色
        [colors addObject:view.backgroundColor ?: [UIColor clearColor]];
    }
    // 调用super
    [super setSelected:selected animated:animated];
    // 修改控件颜色
    for (int i = 0; i < subViews.count; i++) {
        subViews[i].backgroundColor = colors[i];
    }
}

- (void)setHighlighted:(BOOL)highlighted animated:(BOOL)animated{

    // 获取 contentView 所有子控件
    NSArray<__kindof UIView *> *subViews = self.contentView.subviews;
    // 创建颜色数组
    NSMutableArray *colors = [NSMutableArray array];
    
    for (UIView *view in subViews) {
        // 获取所有子控件颜色
        [colors addObject:view.backgroundColor ?: [UIColor clearColor]];
    }
    // 调用super
    [super setHighlighted:highlighted animated:animated];
    // 修改控件颜色
    for (int i = 0; i < subViews.count; i++) {
        subViews[i].backgroundColor = colors[i];
    }
}
```
Swift: 

```swift
override func setSelected(_ selected: Bool, animated: Bool) {
       
        // 获取 contentView 所有子控件
        
        // 创建颜色数组
        let colors = NSMutableArray.init();
        // 获取所有子控件颜色
        for view in self.contentView.subviews {
            
            if view.backgroundColor != nil {
                
                 colors .add(view.backgroundColor!);
            }else{
            
                 colors .add(UIColor.clear);
            }
        }
        // 调用super
         super.setSelected(selected, animated: animated)
       
        // 修改控件颜色
        for i in 0..<self.contentView.subviews.count{
        
            self.contentView.subviews[i].backgroundColor = colors[i] as? UIColor;
        }
    }
    
    override func setHighlighted(_ highlighted: Bool, animated: Bool) {
        
        // 获取 contentView 所有子控件
        
        // 创建颜色数组
        let colors = NSMutableArray.init();
        // 获取所有子控件颜色
        for view in self.contentView.subviews {
            
            if view.backgroundColor != nil {
                
                colors .add(view.backgroundColor!);
            }else{
                
                colors .add(UIColor.clear);
            }
        }
        // 调用super
        super.setHighlighted(highlighted, animated: animated);
        
        // 修改控件颜色
        for i in 0..<self.contentView.subviews.count{
            
            self.contentView.subviews[i].backgroundColor = colors[i] as? UIColor;
        }
    }
```
四、Cell的重用机制和刷新

1.关键字:

	self.tableView.visibleCells 可见的数组,数组中放置屏幕上可见的UITableViewCell对象

2.如果不同重用标志符reuseIdentifier会发生什么？

	tableView每一次显示row时，都会配置一个全新的cell,会不断的开辟内存，这将非常消耗资源，并且影响tableView滚动的效率，导致用户体验很差。

3.重用   

``` objectivec
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath  
{  
    static NSString *Cell = @"MyCell";  
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:Cell];  
    if (!cell) {  
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:Cell];  
    }  
    return cell;  
} 

```
解析:

	• dequeueReusableCellWithIdentifier方法就是去一个队列里面需找有没有相同ID的的Cell:
			如果有，就提出来重用。
			如果没有就跳进if里面去创建。所以我们在if里面创建的时候，不会改变的内容都可以在里面创建，这样就只用创建一次。需要改变的内容我们就放到if后面去写。
	• dequeueReusableCellWithIdentifier方法 就是用来创建几个有限的cell。重用已经创建的Cell， 只需要填充不同的数据即可。



4.刷新
	
	对于只有少数Cell内容变化的，可以针对性的去刷新对应的cell，可以提高效率；
	reloadData会刷新整个tableView；

``` objectivec
UITableView刷新方法:
- (void)beginUpdates;  
- (void)endUpdates;  
 
- (void)insertSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation;  
- (void)deleteSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation;  
- (void)reloadSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation;  
   
- (void)insertRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation: (UITableViewRowAnimation)animation;  
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation: (UITableViewRowAnimation)animation;  
- (void)reloadRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
- 
```
eg:刷新某行代码

	NSIndexPath *row_1=[NSIndexPath indexPathForRow:1 inSection:0];
	NSArray *indexArray=[NSArray arrayWithObject:row_1];
	[tableView reloadRowsAtIndexPaths:indexArray withRowAnimation:UITableViewRowAnimationAutomatic];
	


参考资料：

1.[UITableViewCell点击, 子控件背景颜色不变](https://blog.csdn.net/lea__dongyang/article/details/72303579)