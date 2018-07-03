---
title: iOS模块-UITableView
date: 2018-03-25 15:59:51
tags: iOS模块
categories: iOS
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
四、Cell的重用机制和刷新<br>
1.为什么重用？

	实现显示和数据分离，通过重用单元格来节省内存；
	避免频繁的alloc和delloc cell对象；
	不重用会重复初始化已经初始化的cell，不断的开辟内存，这将非常消耗资源；
	影响tableView滚动的效率，出现卡顿现象，导致用户体验很差；
2.重用原理:<br>
![cell](cell.png)
	
	可变数组visibleCells - readonly，保存当前显示的Cells(UITableViewCell);
	重用池：存储 有 重用标志符identifier 的cell
	
	初始显示：Cell一定未创建
		重用池中无数据;
		cell = nil;
		创建cell,下一次创建Cell时，会将上一次创建的cell放入 visibleCells 中
			创建cell数量问题:  -- 待研究
				n - tableView高度能显示 Cell 的个数
				row - 每组需要显示的行数(定义) numberOfRowsInSection (row ! = 0)
			当row < 2n + 1时,  创建cell的数量: row,  visibleCells个数: row - 1;
			当row >= 2n + 1时, 创建cell的数量: 2n+2, visibleCells个数: 2n + 1;
    滚动显示：
   		通过 dequeueReusableCellWithIdentifier 方法从重用池 获取有对应标志符 identifier 的cell；
   			若cell存在
   			若cell = nil，则创建一个新的cell
		显示在屏幕上，同时放入 visibleCells 数组最后位置，是否移除 visibleCells 中第一个位置的数据，根据屏幕显示而定；
			若 visibleCells 有移除cell，则放入重用池中
3.Cell的差异性问题<br>	
情形描述:
	
	在indexPath.row = 1 设置Cell imageView 的背景颜色，可能导致其他 row 也能看见 设置Cell imageView 的背景颜色
解决方法：

	方法一:针对每个cell设置一个tag，对cell进行差异性判断：
		cell.tag = indexPath.row;
		if(cell.tag ==1){
			...
		}else{
			...
		}
	
	方法二:将cell的identifier设为唯一的, 保证了唯一性
		NSString *identifier = [NSString stringWithFormat:@"%ld%ldcell", indexPath.section, indexPath.row];
	   UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:identifier forIndexPath:indexPath];
	
	方法三:不使用重用机制 - 强烈建议不使用，此方法会造成内存大量使用尤其数据较大时
		UITableViewCell *cell = [tableView cellForRowAtIndexPath:indexPath];
	
	方法四:删除将要重用的cell上的所有子视图, 得到一个没有内容的cel
		[cell.contentView.subviews makeObjectsPerformSelector:@selector(removeFromSuperview)];
tips:
	
	重用池取出来的Cell，可能已经捆绑过数据或者加载过子视图，所以要做必要的数据和视图清除处理	
4.刷新
	
	对于只有少数Cell内容变化的，可以针对性的去刷新对应的cell，可以提高效率；
	reloadData会刷新整个tableView；
	
	reloadData刷新：
		reloadData 调用后
			将 visibleCells 数组中所有数据 移动到 重用池，然后清空 visibleCells；
		cellForRowAtIndexPath调用后
			将 重用池 中的Cell数据取出，放入到 visibleCells 中，显示数据；
	
	reloadRowsAtIndexPaths 刷新:
		
		reloadRowsAtIndexPaths 调用时，
		若 重用池 为空，则在cellForRowAtIndexPath调用后，新创建cell，并且将创建的cell 放入 visibleCells 中，将最底层 visibleCells 中cell数据移除到 重用池；
		若 重用池 取到相关数据， 将取到的数据放入 放入 visibleCells 中对应位置的cell数据，并将其移除到 重用池；
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
五、tableview有footerView时，最后一个cell不显示分割线问题   
OC:

```	objectivec
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
```
Swift:

```swift
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
```
六、TableViewCell优化			
1.使用不透明视图(提高渲染速度)		

	tableViewCell及子视图的opaque = YES（不透明属性，默认yes），alpha = 1；
	不重复创建不必要的Cell，充分利用重用机制；
		cell被重用时，内容不会不会自动清除，可以调用 setNeedDisplayRect: 或 setNeedsDisplay方法；
	尽量减少子视图的数目，自定义除外；
		cell.selectionStyle = UITableViewCellSelectionStyleNone;//设置选中样式
	不要做多余的绘制工作；
	预渲染图像；
	不要阻塞主线程；
七、UITableView 代理 执行的顺序
	
	self.tableView.delegate = UITableViewDelegate
	self.tableView.dataSource = UITableViewDataSource
	
	DataSource
	1.确定组数:...numberOfSectionsInTableView...
	2.确定每组行数:...numberOfRowsInSection...
	3.创建Cell内存:...cellForRowAtIndexPath... 
		每次执行(初始和刷新) cellForRowAtIndexPath 方法后，均会执行步骤4的 heightForRowAtIndexPath 方法；
		因此若cell高度固定，采用 tableView.rowHeight = 50 这种方式更好
		
	Delegate
	4.确定行高:...heightForRowAtIndexPath...
	5.确定Header高度:...heightForHeaderInSection...
	6.确定Footer高度:...heightForFooterInSection...
	7.创建HeaderView:...viewForHeaderInSection...
	8.创建FooterView:...viewForFooterInSection...


参考资料:<br>
1.[UITableViewCell点击, 子控件背景颜色不变](https://blog.csdn.net/lea__dongyang/article/details/72303579)<br>
2.[UITableViewCells重用机制](https://www.jianshu.com/p/1e36643a1aa9)<br>
3.[iOS - 40题](https://blog.csdn.net/qq_30513483/article/details/79723088)