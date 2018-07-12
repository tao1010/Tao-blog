---
title: iOS模块-RunTime
toc: true
date: 2017-03-02 09:51:59
tags: iOS模块
categories: iOS
---

1.概念

<!-- more -->

	runtime是OC底层的一套C语言的API,编译器最终都会将OC代码转化为运行时代码；
	通过终端命令编译.m文件 clang -rewirte-objc xxx.m可以看到编译后的xxx.cpp(C++文件)。
	调用方法本质就是发消息；
	API：引入<objc/runtime.h>或<objc/message.h>
2.作用
	
	动态交换两个方法的实现(特别是系统自带的方法)；
	动态添加对象的成员变量和成员方法；
	获取某个类的所有成员方法、所有成员变量；
3.runtime的运用场景

	将某些OC代码转为运行时代码，探究底层，比如block的实现原理；
	拦截系统自带的方法调用（Swizzle 黑魔法），比如拦截imageNamed:、viewDidLoad、alloc；
	实现分类(类目)也可以增加属性；
		NSObject 添加分类 - 统计创建的对象个数
		UIViewController 添加分类 - 统计创建控制器个数
		原有控件或模块 添加功能
	实现NSCoding的自动归档和自动解档；
	实现字典和模型的自动转换。
4.runtime消息机制的调用流程

	消息机制原理：对象根据方法编号SEL去映射表查找对应的方法实现;
5.案例      
交换类方法

```objectivec
//导入 头文件
#import <objc/runtime.h>

//获取类方法 Method class_getClassMethod(Class cls , SEL name)
Method m1 = class_getClassMethod([WPMyCotroller class], @selector(method1));
Method m2 = class_getClassMethod([WPMyCotroller class], @selector(method2));

//获取对象方法 Method class_getInstanceMethod(Class cls , SEL name)
Method m3 = class_getInstanceMethod([WPMyCotroller class],@selector(method3));

//交换方法的实现 void method_exchangeImplementations(Method m1 , Method m2)
method_exchangeImplementations(m1, m2);//类方法 <--> 类方法
[WPMyCotroller method1]; //实际执行的是 m2对应的方法
/**
method_exchangeImplementations(m1, m3);//类方法 <--> 对象方法
[WPMyCotroller method1]; //实际执行的是 m3对应的方法

```
拦截系统方法  imageNamed: 

	步骤：
	1.创建类目(分类)
	2.在 类目.h文件 添加自定义方法;
	3.在 类目.m文件 导入<objc/runtime.h> 头文件，实现自定义方法和 在+(void)load{交换方法}
	4.在 使用文件中导入类目头文件，使用系统方法即可

```objectivec
//创建类目
UIImage+XT.h	

@interface UIImage (XT)

+ (UIImage *)xt_imageNamed:(NSString *)name;
	
@end
	
UIImage+XT.m

#import "UIImage+XT.h"
#import <objc/runtime.h>

@implementation UIImage (XT)

+ (UIImage *)xt_imageNamed:(NSString *)name{
    
    NSLog(@"runtime 重写系统方法...");
    //用自定义方法调用系统方法(在load中交换了方法) [UIImage xt_imageNamed:name]
    return [UIImage xt_imageNamed:name];
}

+ (void)load{
    
    //获取两个类的类方法
    Method method1 = class_getClassMethod([UIImage class],@selector(imageNamed:));
    Method method2 = class_getClassMethod([UIImage class], @selector(xt_imageNamed:));
    method_exchangeImplementations(method1, method2);
}

@end

//使用 imageNamed 方法
#import "UIImage+XT.h"

//系统方法调用 自定义方法 xt_imageNamed(runtime 交换了方法)
UIImage *image = [UIImage imageNamed:@"home"];

```
给类目(分类)添加属性

	步骤:
	1.创建分类
	2.在分类的.h文件添加属性
	3.在.m文件导入<objc/runtime.h> 头文件，并且重写set、get的实现方法
		
		set方法：将值value 跟对象object 关联起来（将值value 存储到对象object 中）
			void objc_setAssociatedObject(id object , const void *key ,id value ,objc_AssociationPolicy policy)
		get方法: 利用参数key 将对象object中存储的对应值取出来
			id objc_getAssociatedObject(id object , const void *key)
		
		参数说明:
			object: 给哪个对象设置属性 
			key:一个属性对应一个key，通过key取出存储的值，key建议用char 节省字节(可以是double、int等)
			value: 给属性设置的值
			policy:存储策略(assign、copy、retain、strong) OBJC_ASSOCIATION_XXX
	4.导入分类头文件使用属性即可

```objectivec
/**

*/
UIViewController+RunTime.h

@interface UIViewController (RunTime)

@property(nonatomic, copy) NSString *haha;

@end

UIViewController+RunTime.m
#import "UIViewController+RunTime.h"
#import <objc/runtime.h>

char hahaKey;

@implementation UIViewController (RunTime)

-(void)setHaha:(NSString *)haha{
    // 将某个值跟某个对象关联起来，将某个值存储到某个对象中
    objc_setAssociatedObject(self, &hahaKey, haha, OBJC_ASSOCIATION_COPY_NONATOMIC);
}

- (NSString *)haha{
    return objc_getAssociatedObject(self, &hahaKey);
}

@end

*/
```
获取一个类的所有成员变量
	
	典型用法：归档和解档
	
	获取某个类的所有成员变量
	Ivar *class_copyIvarList(Class cls , unsigned int *outCount)
	参数说明:
		cls: 哪个类
		outCount: 存放属性个数(一个接受值的地址)
	获取成员变量名字
	const char *ivar_getName(Ivar v)
	
	获取成员变量的类型
	const char *ivar_getTypeEndcoding(Ivar v)
	
```objectivec
	//导入头文件
	#import <objc/runtime.h>

	unsigned int outCount = 0;
    Ivar *ivars = class_copyIvarList([WPMyCotroller class], &outCount);
    //遍历成员变量
    for (int i = 0; i < outCount; i ++) {
        //取出i位置对应的成员变量
        Ivar ivar = ivars[i];
        const char *name = ivar_getName(ivar);
        const char *type = ivar_getTypeEncoding(ivar);
        NSLog(@"成员变量:%s 成员变量类型:%s",name,type);
    }
    //释放内存!释放内存!释放内存!释放内存!
    free(ivars);
```

获取所有属性重写归档解档方法

	第三方库：MJExtension  中定义了归档解档的宏，一句宏搞定
	
	步骤：
	1.创建NSObject类目（分类）
	2.在类目 .h文件定义方法
	3.在类目 .m文件导入<objc/runtime.h> 头文件，并且写方法的实现方法
	4.其他地方运用
```objectivec
NSObject+XT.h

@interface NSObject (XT)

- (NSArray *)ignoredNames;
- (void)encode:(NSCoder *)coder;
- (void)decode:(NSCoder *)decoder;
@end

NSObject+XT.m
#import <objc/runtime.h>

@implementation NSObject (XT)

- (void)decode:(NSCoder *)decoder {
    // 一层层父类往上查找，对父类的属性执行归解档方法
    Class c = self.class;
    while (c &&c != [NSObject class]) {
        
        unsigned int outCount = 0;
        Ivar *ivars = class_copyIvarList(c, &outCount);
        for (int i = 0; i < outCount; i++) {
            Ivar ivar = ivars[i];
            NSString *key = [NSString stringWithUTF8String:ivar_getName(ivar)];
            
            // 如果有实现该方法再去调用
            if ([self respondsToSelector:@selector(ignoredNames)]) {
                if ([[self ignoredNames] containsObject:key]) {
                		continue;
                }
            }
            
            id value = [aDecoder decodeObjectForKey:key];
            [self setValue:value forKey:key];
        }
        free(ivars);//释放内存
        c = [c superclass];
    }
    
}

- (void)encode:(NSCoder *)coder {
    // 一层层父类往上查找，对父类的属性执行归解档方法
    Class c = self.class;
    while (c &&c != [NSObject class]) {
        
        unsigned int outCount = 0;
        Ivar *ivars = class_copyIvarList([self class], &outCount);
        for (int i = 0; i < outCount; i++) {
            Ivar ivar = ivars[i];
            NSString *key = [NSString stringWithUTF8String:ivar_getName(ivar)];
            
            // 如果有实现该方法再去调用
            if ([self respondsToSelector:@selector(ignoredNames)]) {
                if ([[self ignoredNames] containsObject:key]){
 	               	continue;
              	}
            }
            
            id value = [self valueForKeyPath:key];
            [aCoder encodeObject:value forKey:key];
        }
        free(ivars);//释放内存
        c = [c superclass];
    }
}
@end

//在解档归档的对象中实现
// 设置需要忽略的属性
- (NSArray *)ignoredNames {
    return @[@"key"];
}

// 在系统方法内来调用我们的方法
- (instancetype)initWithCoder:(NSCoder *)aDecoder {
    if (self = [super init]) {
        [self decode:aDecoder];
    }
    return self;
}

- (void)encodeWithCoder:(NSCoder *)aCoder {
    [self encode:aCoder];
}
```

获取所有属性来进行字典转模型
	
	字典转模型三种情况:
		当字典的key和模型的属性匹配不上;
		模型中嵌套模型(模型属性是另外一个模型对象)
			1.使用runtime 方法判断模型对象类型 ivar_getTypeEncoding()
			2.对嵌套的模型对象 进行字典转模型(递归) - 注意排除系统的对象类型如NSString等
		数组中装着模型(模型的属性是一个数组,数组中是一个个模型对象)
			获取装有 模型的数组 的属性;
			遍历数组中的每个模型并转为字典;
				声明方法返回模型的类型;
	方法一：KVC(局限性:模型属性和键值对 对应不上会crash,setValue:forUndefineKey:可防止crash)
	方法二：runtime
```objectivec

#import <objc/runtime.h>

@implementation NSObject (XT)

// 获取数组中都是什么类型的模型对象
- (NSString *)arrayObjectClass{
	
	//实现???
}

- (void)setDict:(NSDictionary *)dict {
    
    Class c = self.class;
    while (c &&c != [NSObject class]) {
        
        unsigned int outCount = 0;
        Ivar *ivars = class_copyIvarList(c, &outCount);
        for (int i = 0; i < outCount; i++) {
            Ivar ivar = ivars[i];
            NSString *key = [NSString stringWithUTF8String:ivar_getName(ivar)];
            
            // 成员变量名转为属性名（去掉下划线 _ ）
            key = [key substringFromIndex:1];
            // 取出字典的值
            id value = dict[key];
            
            // 如果模型属性数量大于字典键值对数理，模型属性会被赋值为nil而报错
            if (value == nil) continue;//？？？？？
            
            // 获得成员变量的类型
            NSString *type = [NSString stringWithUTF8String:ivar_getTypeEncoding(ivar)];
            
            // 如果属性是对象类型
            NSRange range = [type rangeOfString:@"@"];
            if (range.location != NSNotFound) {
                // 那么截取对象的名字（比如@"Dog"，截取为Dog）
                type = [type substringWithRange:NSMakeRange(2, type.length - 3)];
                // 排除系统的对象类型
                if (![type hasPrefix:@"NS"]) {
                    // 将对象名转换为对象的类型，将新的对象字典转模型（递归）
                    Class class = NSClassFromString(type);
                    value = [class objectWithDict:value];
                    
                }else if ([type isEqualToString:@"NSArray"]) {
                    
                    // 如果是数组类型，将数组中的每个模型进行字典转模型，先创建一个临时数组存放模型
                    NSArray *array = (NSArray *)value;
                    NSMutableArray *mArray = [NSMutableArray array];
                    
                    // 获取到每个模型的类型
                    id class ;
                    if ([self respondsToSelector:@selector(arrayObjectClass)]) {
                        
                        NSString *classStr = [self arrayObjectClass];
                        class = NSClassFromString(classStr);
                    }
                    // 将数组中的所有模型进行字典转模型
                    for (int i = 0; i < array.count; i++) {
                        [mArray addObject:[class objectWithDict:value[i]]];
                    }
                    
                    value = mArray;
                }
            }
            
            // 将字典中的值设置到模型上
            [self setValue:value forKeyPath:key];
        }
        free(ivars);
        c = [c superclass];
    }
}

+ (instancetype )objectWithDict:(NSDictionary *)dict {
    NSObject *obj = [[self alloc]init];
    [obj setDict:dict];
    return obj;
}

@end
```
参考资料:     
1.[OC中的RunTime运用](https://www.jianshu.com/p/ab966e8a82e2)     
2.[消息机制](https://blog.csdn.net/li15809284891/article/details/54767504)    