---
title: Windows Flutter安装和运行
categories: Flutter
abbrlink: 53312
date: 2019-08-01 16:34:21
---
## Flutter介绍
Flutter是谷歌的移动UI框架，可以快速在iOS和Android上构建高质量的原生用户界面。 Flutter可以与现有的代码一起工作。在全世界，Flutter正在被越来越多的开发者和组织使用，并且Flutter是完全免费、开源的。

## Windows环境要求
- Java环境（配置环境变量）；
- [Android Studio](https://developer.android.google.cn/studio)（主要安装模拟器和自带环境）；
- [Vscode](https://code.visualstudio.com/)（“宇宙之强”编辑器）；
- Git。

## Flutter安装
官网安装：https://flutter.dev/docs/get-started/install
or
Github（有点大，可能要多试几次）：`$ git clone -b beta https://github.com/flutter/flutter.git`

## 测试
是否安装完成：
```
$ flutter --help
```
查看是否需要安装任何依赖项来完成设置：
```
$ flutter doctor
```
提示：有报红的一个一个的解决（打×的要解决，!号的可以不用）。

## Vscode插件安装
- flutter
- dart
安装完重启vscode，
在编辑器的左下角的设置点击进入 “命令面板”，在命令面板中点击flutter：new project [项目名称]

## 启动

在终端命令行里面输入 ： `$ flutter run`  等待程序的执行，完成`hello world`

```
R 键重新启动
r 键热重载
q 退出
p 显示网格
P 显示帧率
o 切换Android与iOS的预览模式
```
