---
title: uniapp 自定义Tabbar探讨
abbrlink: 53095
date: 2020-02-13 15:46:37
tags: 
	- uniapp
---
## 为什么要用自定义tabbar？原生自带的满足不了吗？
答：是的，基于两点需求，1.显示红点数量样式修改；2.底部弹框被原生tabbar挡住。
## 尝试过多种方案，也没有达到预期。
### 1.tabbar组件形式
每个tabbar页面都引用自定义tabbar组件，传当前页面的参数作为选中当前项。
不足：每次切换tabbar页面，组件也重新加载，达不到效果。
### 2.页面组件形式
tabbar页面全部作为组件形式，先新建一个index.vue页面，引入自定义tabbar，引入全部tabbar页面组件，为tabbar按钮添加点击事件，判断显示哪个页面组件。
```
<template>
	<div>
		<page-work v-if="current == 0" />
		<page-maillist v-if="current == 1" />
		<page-order v-if="current == 2" />
		<page-my v-if="current == 3" />
		<sl-tabbar  @tabbarBack="tabbarBack" :current="current" />
	</div>
</template>

<script>
import pageWork from '../../pages/work/index.vue'
import pageMaillist from '../../pages/maillist/index.vue'
import pageOrder from '../../pages/order/index.vue'
import pageMy from '../../pages/my/index.vue'
import slTabbar from '@/components/tabbar/index.vue'
export default {
	components: {
		pageWork,
		pageMaillist,
		pageOrder,
		pageMy,
		slTabbar,
	},
	data() {
		return {
			current: 0
		}
	},
	methods: {
		tabbarBack(i, idx) {
			this.current = idx;
		}
	}
```
不足：页面的特性已经丧失，页面生命周期不复存在，严重影响原有页面逻辑。
### 3.按需显示隐藏原生tabbar
使用`uni.showTabBar()`和`uni.hideTabBar()`动态显示隐藏原生tabbar，弹框的时候隐藏原生tabbar，并同时显示自定义tabbar，否则反之。
不足，切换时有闪烁效果，不理想。
