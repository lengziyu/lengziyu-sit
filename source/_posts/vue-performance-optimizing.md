---
title: vue项目优化
abbrlink: 9900
tags:
  - Vue
  - webpack3
  - vue-cli3
categories: Vue
date: 2020-08-26 17:14:45
---

## 项目主要针对*首屏加载*做处理，至少能提升70%。
- vue-cli3
- webpack3

## cdn配置
cdn优点：有利于减少打包体积，可以缓解我们服务器的压力，原理是将我们的压力分给其他服务器点。

```
// vue.config.js
module.exports = {
  chainWebpack: (config) =>{
	config.externals({
		'vue': 'Vue',
		'vuex': 'Vuex',
		'vue-router': 'VueRouter',
		'element-ui': 'ELEMENT'
	})
}
```
相对稳定的cdn有[bootcdn](https://www.bootcdn.cn)、[unpkg](https://unpkg.com)。
```
// public/index.html

<!DOCTYPE html>
	...
	<script src="https://unpkg.com/vue@2.6.10/dist/vue.min.js"></script>
	<script src="https://unpkg.com/vue-router@3.0.7/dist/vue-router.min.js"></script>
	<script src="https://unpkg.com/vuex@3.1.2/dist/vuex.js"></script>
  </body>
</html>
```
去掉import，如：
```
// import Vue from 'vue'
// import Router from 'vue-router'
```
去掉`Vue.use(XXX)`，如：
```
// Vue.use(Router)
```

## 服务器端开启Gzip
开启 nginx 服务端 gzip性能优化。找到nginx配置文件在 http 配置里面添加如下代码，然后重启nginx服务即可。
```
http:{ 
      gzip on; 
      gzip_static on;
      gzip_buffers 4 16k;
      gzip_comp_level 5;
      gzip_types text/plain application/javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg 
                 image/gif image/png;

}
```
加载时长至少减少一半以上。

## 体积分析
先安装`webpack-bundle-analyzer`
```
npm i webpack-bundle-analyzer -S
```
配置如下：
```
chainWebpack: (config) =>{
  // 打包文件分析
  config
	.plugin('webpack-bundle-analyzer')
	.use(require('webpack-bundle-analyzer').BundleAnalyzerPlugin)
}
```
运行`npm run serve`启动就会自动打开浏览器页面`http://127.0.0.1:8888`。

对于用的少，体积又大的可以舍弃或者更换更小体积的库。

## 	去掉预渲染
该版本默认会自带预渲染，这样大大增加首屏渲染的加载时间。
```
chainWebpack: (config) =>{
	// 移除prefetch插件
	config.plugins.delete('prefetch')
}
```
如果部分需要预渲染的话，可以在`router`里配置：
```
const MenuTree = () => import(/* webpackChunkName: "MenuTree" */ './module/menu-tree');
···
components: { MenuTree }
```

## 小优化
去掉线上的打印，关闭sourceMap。
```
...
	chainWebpack: (config) =>{
	      /** 去掉console.log debugger sourceMap*/
	      config.optimization.minimizer([
	        new UglifyJsWebpackPlugin({
	          /**这个 sourceMap注释掉，默认就是置为false.(写为false 也是可以的)。
	           * 反之设为true 是生效的。
	           * 故在官方的配置(productionSourceMap: false)就可以注释掉了*/
	          sourceMap: false,
	          uglifyOptions: {
	            warnings: false,
	            compress: {
	              drop_console: true,
	              drop_debugger: true
	            }
	          }
	        })
	      ]);
	}
```