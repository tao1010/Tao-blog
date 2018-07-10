---
title: iOS库-Framework开发
toc: true
date: 2017-03-20 15:19:06
tags: iOS第三方库
categories: iOS
---

步骤

<!-- more -->

1.创建FrameWork项目
![create](create.png)     
![file](file.png)     

2.开发对应功能的模块
![import](import.png)

	导入依赖框架
	设置暴露接口
		主要设置Public和Private
3.打包框架    
方法一：脚本 - 推荐
![shell](shell.png)    
脚本内容:

```shell
if [ "${ACTION}" = "build" ]

then
	
INSTALL_DIR=${SRCROOT}/Products/${PROJECT_NAME}.framework
	
DEVICE_DIR=${BUILD_ROOT}/${CONFIGURATION}-iphoneos/${PROJECT_NAME}.framework
	
SIMULATOR_DIR=${BUILD_ROOT}/${CONFIGURATION}-iphonesimulator/${PROJECT_NAME}.framework
	
if [ -d "${INSTALL_DIR}" ]
	
then
	
rm -rf "${INSTALL_DIR}"
	
fi
	
mkdir -p "${INSTALL_DIR}"
	
cp -R "${DEVICE_DIR}/" "${INSTALL_DIR}/"
	
#ditto "${DEVICE_DIR}/Headers" "${INSTALL_DIR}/Headers"
	
lipo -create "${DEVICE_DIR}/${PROJECT_NAME}" "${SIMULATOR_DIR}/${PROJECT_NAME}" -output "${INSTALL_DIR}/${PROJECT_NAME}"
	
#open "${DEVICE_DIR}"
	
open "${SRCROOT}/Products"
	
fi
```

	分别在32位 - iPhone5 和 64位 - iPhone5s及以上 编译运行即可
方法二: 分别编译真机和模拟器 在合并FrameWork框架即可     
	
	终端输入:
	lipo -create 模拟器.framework文件路径 真机.framework文件路径 -output 生成framework文件路径/xxx.framework
4.使用
	
	将步骤3打包的FrameWork框架导入项目中，导入头文件；
	编译器提前编译这个二进制文件：
		进入General->Embedded Binaries,将加入的Framework添加上去。
参考资料:     
1.[制作Framework框架](https://www.jianshu.com/p/d11f4c85f7b5)   
2.[SDK开发-framework](https://www.jianshu.com/p/65b1c1326c50)