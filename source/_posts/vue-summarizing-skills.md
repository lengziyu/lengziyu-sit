---
title: uniapp 开发记录
abbrlink: 48671
date: 2019-10-18 19:53:40
tags: 
	- uniapp
---
# 基本
单位用rpx，布局用Flex

# 打包
### 云端打包
**安卓端&iOS**
云端打包流程：打开HBuilderX，顶部菜单【发行->原生App-云打包】，选打自定义调试基座，等待打包成功，原生App-查看云打包状态，编辑器底部打印台会显示最近打包的地址，点击下载即可。
注：iOS端需要苹果开发证书。
### 本地离线打包
**安卓**

用官方HBuilder-Hello的项目，
参考这个：[uni-app用AndroidStudio打包apk文件](https://www.jianshu.com/p/e41d80c0cbc2)

# h5与app区别
1. CSS`background-size`属性，写在CSS里h5端生效，但是app端无效：
```
// app无效
uni-swiper-item{
        background-size: 100% 100%;
}
```
解决方法：就是在标签动态添加`backgroundSize`属性：
```
<uni-swiper-item
:style="{ backgroundImage: 'url('+ banner.url +')', backgroundSize: 'cover' }"
>
</uni-swiper-item>
```
