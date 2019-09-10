---
title: MongoDB 学习笔记
tags: mongodb
abbrlink: 6031
date: 2016-11-05 17:15:25
categories:
---
简介：MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。在高负载的情况下，添加更多的节点，可以保证服务器性能。旨在为WEB应用提供可扩展的高性能数据存储解决方案。
<!--more-->
## 常用shell
开启服务：
```
$ net start MongoDB
```
停止服务：
```
$ net stop MongoDB
```
删除服务：
```
$ "D:\MongoDB\bin\Server\3.2\mongod.exe" --remove
```
## 简单操作

命令用于查看当前操作的文档（数据库）：
```
> db          //显示当前数据库对象或集合
> show dbs     //显示所有数据的列表
> use test    //运行"use"命令，可以连接到一个指定的数据库，这里连接到test数据库，如果连接的不存在，则自动新建
> db.runoob.insert({"name":"lengziyu"}) //向 runoob 数据库插入一些数据
> db.dropDatabase()   //删除当前数据库

```
