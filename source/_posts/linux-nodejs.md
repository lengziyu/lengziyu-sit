---
title: '在 Linux 服务器上安装 node [搭建内部npm]'
tags:
  - linux
  - node
abbrlink: 24595
date: 2016-11-03 20:37:40
categories:
---
这周的工作任务是在 Linux 上搭建私人内部 npm，虽然网上很多教程，但是很多我缺踩了很多未知的坑，以此记录一下。
<!--more-->
**搭建私人npm大概有一下方法：**

- 用 [sinopia](https://github.com/rlidwka/sinopia) 安装；
- 淘宝  [cnpm](https://github.com/cnpm/cnpmjs.org) 安装。

`cnpm` 涉及到数据库mysql，弃之。

## 安装 node
因为我的linux服务器无法连接外网，所以只能先连接外网下载好[node](https://nodejs.org/dist/latest-v4.x/)源码，然后再用sftp上传到服务器上，这里我下载的是：node-v4.6.1-linux-x64.tar.gz。
上传成功后，cd到该目录下，进行解压，并且cd进去：
```Shell
$ tar xvf node-v4.6.1-linux-x64.tar.gz
$ cd node-v4.6.1-linux-x64
```
安装：
```Shell
./configure
```
测试是否安装成功：
```
./bin/node -v
```
此时成功的话会出现4.6.1。
接下来设置为全局：
```
ln -s /home/node-v4.6.1-linux-x64/bin/node /usr/local/bin/node
ln -s /home/node-v4.6.1-linux-x64/bin/npm /usr/local/bin/npm
```
这里/home/这个路径是你自己放的，你将node文件解压到哪里就是哪里。
```
$ node -v
```
出现4.6.1，OK。此时设置全局完成。
