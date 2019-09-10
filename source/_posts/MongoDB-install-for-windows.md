---
title: 在win7上安装MongoDB
tags: mongodb
abbrlink: 41631
date: 2016-11-05 17:14:45
categories:
---
最近用了一星期linux，感觉需要学习一门后端语言和一个数据库。暂时选定nodejs和MongoDB。
<!--more-->
## 下载安装
官网下载地址：https://www.mongodb.com/download-center#community
老是提示下载不了，附百度云盘下载：http://pan.baidu.com/s/1c2KFwGC 密码:7vu0
安装过程中我选择`D:\MongoDB`安装目录，傻瓜式安装完成。

**打开管理员命令提示**
需要通过管理员模式的命令提示符，来执行安装命令。
管理员打开cmd输入以下命令：
在MongoDB文件下创建`data\db`：
```
mkdir D:\MongoDB\data\db
```
MongoDB需要数据目录来存储所有的数据，其默认的数据目录为`\data\db` ，可以通过mongod.exe --dbpath命令来指定MongoDB的数据目录。例如：
```
D:\MongoDB\Server\3.2\bin\mongod.exe --dbpath D:\MongoDB\data\db
```
看到上面的提示底部出现waiting for connections 字样，则表示dbpath配置完成，且MongoDB启动成功。
而且此时打开资源管理器，进入MongoDB的dbpath目录，可以看见本地确实初始化数据库了。

## 运行MongoDB
通过运行mongo.exe启动MongoDB。例如：
```
D:\MongoDB\Server\3.2\bin\mongo.exe
```
命令行窗口显示如下内容：
```
2016-11-05T17:42:53.821+0800 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
MongoDB shell version: 3.2.10
commecting to: test
```
窗口中可以看到当前MongoDB shell的版本，及此时连接的数据库。

## 设置全局path变量
在计算机右键 ==> 属性 ==> 高级系统设置 ==> 环境变量 ==> 选择Path编辑 ==>
在最后面加`;`然后添加你安装的路径:`D:\MongoDB\Server\3.2\bin\mongo.exe`
确定保存。
然后在cmd上输入：
```
mongo
```
此时会出现：
```
2016-11-05T17:42:53.821+0800 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
MongoDB shell version: 3.2.10
connecting to: test
```
恭喜你，说明安装完成！
