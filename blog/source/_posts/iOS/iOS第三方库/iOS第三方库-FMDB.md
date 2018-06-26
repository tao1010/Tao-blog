---
title: iOS第三方库-FMDB
toc: true
date: 2018-06-26 13:23:13
tags: iOS第三方库
categories: iOS
---

一、FMDB基本信息<br>

<!-- more -->

1.版本信息
	
	pod 'FMDB', '~> 2.7'
2.结构<br>
![fmdb](fmdb.png)<br>

	FMDatabase
		一个 FMDatabase 对象代表一个单独的SQLite数据库(不是表)，用来执行SQL语句(增、删、改、查);
		
	FMDatabaseAdditions
		扩展 FMDatabase 类 新增对查询结果至返回单个值的方法进行简化,对表、列是否存在、版本号，校验SQL等功能；		
	FMResultSet
		使用 FMDatabase 执行查询后的结果集；

	FMDatabaseQueue
		用于多线程中执行多个查询或更新，使用串行队列，线程安全的；

	FMDatabasePool
		使用任务池，对多线程提供支持，优先选择FMDatabaseQueue；
3.简介
	
	FMDB是iOS中对 libsqlite3.tbd 库 的封装；
		更加面向对象；
		非常轻量、灵活；
		线程安全；
    	存在跨平台的局限性；

二、libsqlite3 和 FMDB<br>
	
	libsqlite3是iOS中纯C语言操作Sqlite数据库的一个库文件;
1.SQLite3 操作数据库需要使用3个步骤：

	1.使用sqlite3_open()函数打开数据库;
	2.使用sqlite3_exec()函数执行非查询的SQL语句:
		创建数据库；
		创建数据表；
		增、删、改、查等操作；
	3.使用SQLite_close()函数释放资源;
2.基本使用对比<br>
数据库操作对象

	C语言方式：
		sqlite3 *db;
	FMDB方式：
		FMDatabase *db;
打开数据库

	//1.获取数据库文件路径
    NSString *doc = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
    NSString *fileName = [doc stringByAppendingPathComponent:@"students.sqlite"];
		    
	C语言方式：
		-(void)openDB{
		    //2.将OC字符串转换为c语言的字符串
		    const char *cfileName = fileName.UTF8String;
		
		    //3.打开数据库文件（如果数据库文件不存在，那么该函数会自动创建数据库文件）
		    int result = sqlite3_open(cfileName, &db);
		    if (result == SQLITE_OK) {//打开成功
		        NSLog(@"成功打开数据库");
		    }else{
		        NSLog(@"打开数据库失败");
		    }
		}
	FMDB方式：	
		-(void)openDB{
			//2、获取数据库连接
		   db = [FMDatabase databaseWithPath:fileName];
		
		    //3、打开数据库连接
		    if ([db open]) {
		        NSLog(@"打开数据库成功");
		    }else{
		        NSLog(@"打开数据库失败");
		    }
		}
创建表格
	
	C语言方式
		-(void)createTable{
			//创建表
		    const char *sql = "CREATE TABLE IF NOT EXISTS t_student(id integer PRIMARY KEY AUTOINCREMENT,name text NOT NULL,age integer NOT NULL);";
		    char *errmsg= NULL;
		    int result = sqlite3_exec(_db, sql, NULL, NULL, &errmsg);
		    if (result==SQLITE_OK) {
		        NSLog(@"创建表成功");
		    }else{
		        NSLog(@"创建表失败---%s",errmsg);
		    }
		}
	FMDB方式
		-(void)createTable{
			BOOL result = [_db executeUpdate:@"CREATE TABLE IF NOT EXISTS t_student (id integer PRIMARY KEY AUTOINCREMENT, name text NOT NULL, age integer NOT NULL);"];
		    if (result) {
		        NSLog(@"创建表格成功");
		    }else{
		        NSLog(@"创建表格失败");
		    }
		}
增加数据

	C语言方式
		-(void)insertData{
			//插入数据
		    for (int i=0; i<10; i++) {
		        //拼接sql语句
		        NSString *name = [NSString stringWithFormat:@"yixiangboy--%d",arc4random_uniform(100)];
		        int age = arc4random_uniform(20)+10;
		        NSString *sql = [NSString stringWithFormat:@"INSERT INTO t_student (name,age) VALUES ('%@',%d);",name,age];
		
		         //执行SQL语句
		        char *errmsg = NULL;
		        sqlite3_exec(db, sql.UTF8String, NULL, NULL, &errmsg);
		        if (errmsg) {//如果有错误信息
		            NSLog(@"插入数据失败--%s",errmsg);
		        }else{
		            NSLog(@"插入数据成功");
		        }
		    }
		}
	FMDB方式
		-(void)insertData{
			for (int i=0; i<10; i++) {
		        NSString *name = [NSString stringWithFormat:@"yixiang-%d",arc4random_uniform(100)];
		        int age = arc4random_uniform(20)+10;
		        BOOL result = [_db executeUpdate:@"INSERT INTO t_student (name, age) VALUES (?, ?);",name, @(age)];
		        if (result) {
		            NSLog(@"插入成功");
		        }else{
		            NSLog(@"插入失败");
		        }
		    }
		}
删除数据

	C语言方式
		-(void)deleteData{
			//删除age小于15的数据
		    NSString *sql = [NSString stringWithFormat:@"DELETE FROM t_student WHERE age<15"];
		    char *errmsg = NULL;
		    sqlite3_exec(_db, sql.UTF8String, NULL, NULL, &errmsg);
		    if (errmsg) {
		        NSLog(@"删除数据失败");
		    }else{
		        NSLog(@"删除数据成功");
		    }
		}
	FMDB方式
		-(void)deleteData{
			 BOOL result = [_db executeUpdate:@"DELETE FROM t_student WHERE age<15"];
		    if (result) {
		        NSLog(@"删除成功");
		    }else{
		        NSLog(@"删除失败");
		    }
		}
更新数据
	
	C语言方式
		-(void)updateData{
			 BOOL result = [_db executeUpdate:@"DELETE FROM t_student WHERE age<15"];
		    if (result) {
		        NSLog(@"删除成功");
		    }else{
		        NSLog(@"删除失败");
		    }
		}
	FMDB方式
		-(void)updateData{
			 BOOL result = [_db executeUpdate:@"UPDATE t_student set age=20 WHERE age>20"];
		    if (result) {
		        NSLog(@"更新成功");
		    }else{
		        NSLog(@"更新失败");
		    }
		}
查询数据
	
	C语言方式
		-(void) queryData{
			 const char *sql = "SELECT id,name,age FROM t_student WHERE age<20";
		    sqlite3_stmt *stmt = NULL;
		
		    //进行查询前的准备工作
		    if(sqlite3_prepare_v2(_db, sql, -1, &stmt, NULL)==SQLITE_OK){//SQL语句没有问题
		        NSLog(@"查询语句没有问题");
		
		        //每调用一次sqlite3_step函数，stmt就会指向下一条记录
		        while (sqlite3_step(stmt)==SQLITE_ROW) {//找到一条记录
		            //取出数据
		            //(1)取出第0个字段的值（int）
		            int ID=sqlite3_column_int(stmt, 0);
		            //(2)取出第一列字段的值(text)
		            const unsigned char *name = sqlite3_column_text(stmt, 1);
		            //(3)取出第二列字段的值(int)
		            int age = sqlite3_column_int(stmt, 2);
		
		            printf("%d %s %d\n",ID,name,age);
		        }
		    }else{
		        NSLog(@"查询语句有问题");
		    }
		}
	FMDB方式
		-(void)queryData{
			 FMResultSet *resultSet = [_db executeQuery:@"SELECT * FROM t_student WHERE age > ?",@(20)];

		    while ([resultSet next]) {
		        int ID = [resultSet intForColumn:@"id"];
		        NSString *name = [resultSet stringForColumn:@"name"];
		        int age = [resultSet intForColumn:@"age"];
		        NSLog(@"%d %@ %d",ID,name,age);
		    }
		}
参考资料:<br>
1.[FMDB-cocoapods](https://cocoapods.org/pods/FMDB)<br>
2.[FMDB和SQLite3对比](https://blog.csdn.net/yixiangboy/article/details/51072699)<br>
3.[FMDB源码阅读](http://www.cnblogs.com/polobymulberry/category/789988.html)