---
title: 公众号用vue开发百度地图
abbrlink: 7031
date: 2019-09-10 23:01:18
tags: 微信公众号
---
## 功能描述
最近用vue在公众号结合百度地图开发，功能是根据公众号定位获取经纬度，再用百度地图显示附近的店铺。由于公众号不能满足业务需求，所以加上百度地图。

## 知识点
- 微信公众号获取经纬度
- 公众号经纬度转换百度经纬度
- vue开发用[vue-baidu-map](https://github.com/Dafrok/vue-baidu-map)开发注意事项
- 百度地图逆/地址解析
- 推荐一些相关的开发/测试工具

## 微信公众号获取经纬度
公众号文档-[获取地理位置接口](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html#36)
前提是公众号的一切东西都设置好，要先调接口获取签名，然后通过`wx.config`安全验证，
```
/**
*  获取公众号经纬度
*  Vue 注册全局函数的方式  
*/
Vue.prototype.wxLocation = (opts) => {
  const ua = window.navigator.userAgent.toLowerCase();
  if (ua.match(/MicroMessenger/i) == 'micromessenger') {
    // 这是一个请求，获取签名
    api.wxSignature({
        url: window.location.href.split('#')[0]
      })
      .then(res => {
        // 安全验证
        wx.config({
          debug: false,
          appId: res.data.appid,
          timestamp: res.data.timestamp,
          nonceStr: res.data.nonceStr,
          signature: res.data.signature,
          jsApiList: [
            'openLocation',
            'getLocation',
          ],
        });

        wx.ready(() => {
          // 获取经纬度
          wx.getLocation({
            type: 'wgs84', // 默认为wgs84的gps坐标，如果要返回直接给openLocation用的火星坐标，可传入'gcj02'
            success: (res)=> {
              var latitude = res.latitude; // 纬度，浮点数，范围为90 ~ -90
              var longitude = res.longitude; // 经度，浮点数，范围为180 ~ -180。
              opts.wxGetLocation(res)
            },
          });
        })
      })
  } else {
    console.log('非微信端，不获取地址！')
  }
},
```
页面调用：
```
created() {
  this.wxLocation({
    wxGetLocation: function(res) {
      console.log(res);
    }
  });
},

```

## 公众号经纬度转换百度经纬度
**目前国内主要有以下三种坐标系：**

- WGS84：为一种大地坐标系，也是目前广泛使用的GPS全球卫星定位系统使用的坐标系。

- GCJ02：又称火星坐标系，是由中国国家测绘局制订的地理信息系统的坐标系统。由WGS84坐标系经加密后的坐标系。

- BD09：为百度坐标系，在GCJ02坐标系基础上再次加密。其中bd09ll表示百度经纬度坐标，bd09mc表示百度墨卡托米制坐标。

非中国地区地图，服务坐标统一使用WGS84坐标。
[百度地图-坐标转换说明](http://lbsyun.baidu.com/index.php?title=jspopular3.0/guide/coorinfo)

我了解的有两种方式转换：
- [文档化-坐标转换服务](http://lbsyun.baidu.com/index.php?title=webapi/guide/changeposition)
- 通过接口转换
下面介绍通过接口的方式转换：
```
http://api.map.baidu.com/ag/coord/convert?from=0&to=4&x=113.540124&y=23.517846
```

其中`from`参数：0代表WGS-84即标准GPS设备返回的坐标, 2代表国测局的标准；`to`参数：4代表百度的坐标；`x`参数：代表纬度latitude；`y`参数：代表经度longitude。

大概会出现跨域问题，我用了vue-cli2配置的`proxyTable`无法解决，后来我用了朋友的跨域应用解决了：
```
https://bird.ioliu.cn/v2?url=http://api.map.baidu.com/ag/coord/convert?from=0&to=4&x=113.540124&y=23.517846
```
转换出来是经过base64加码的，这样子：
```
{"error":0,"x":"MTEzLjU1MTgxMDg2MDE5","y":"MjMuNTIxMjM0OTM0OTE0"}
```
用`js-base64`解码：
```
let Base64 = require('js-base64').Base64;
...
axios.get('https://bird.ioliu.cn/v2?url=http://api.map.baidu.com/ag/coord/convert?from=0&to=4&x=113.540124&y=23.517846')
      .then(res){
        var longitude = Base64.decode(res.x);
        var latitude = Base64.decode(res.y);
      }

```
此时，转换经纬度大功告成！

## vue开发用[vue-baidu-map](https://github.com/Dafrok/vue-baidu-map)开发注意事项

安装就不说了，文档都有，说说遇到的坑吧。
问题1.
   自定义覆盖物，定义的按钮不触发事件，查看DOM，发现触发了地图蒙层，
解决：
引入
```
<script src="http://api.map.baidu.com/library/EventWrapper/1.2/src/EventWrapper.js" charset="utf-8"></script>
```
使用：
```
// html
<baidu-map class="bm-view" ref="baiduMap" :zoom="15" @click="overlay">

</baidu-map>

// js
methods: {
  overlay(event) {
    BMapLib.EventWrapper.addDomListener(this.$refs.baiduMap.$el, "touchend", (e)=>{
      alert('点击触发')
    }, false)
},
```
问题2.
获取百度地图实例`BMap`，
解决：
```
// html
<baidu-map class="bm-view" ref="baiduMap"  @ready="mapHandler" :zoom="15">

</baidu-map>

// js
methods: {
  // 初始化
  mapHandler(e) {
    console.log(e) // { BMap: xx, map: xx }
  },
},
```
## 推荐一些相关的开发/测试工具
- [查询经纬度](http://api.map.baidu.com/lbsapi/getpoint/index.html)
