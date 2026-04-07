<template>
	<view class="container">
		<image :src="imgUrl" class="full-img"></image>
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'
	import {
		onLoad
	} from '@dcloudio/uni-app'

	const imgUrl = ref('')

	onLoad((options) => {
		// 接收URL参数里的img_path，比如跳转路径是 /pages/xxx/xxx?img_path=xxx
		console.log(options.img_path)
		imgUrl.value  = '/static/' + options.img_path + ".jpg"
		// 动态设置图片路径，如果参数是完整路径可以直接赋值，这里做兼容处理
		// imgUrl.value = options.img_path || '/static/GZCY.jpg'
	})
</script>

<style>
	/* 重置页面默认样式，避免留白 */
	page {
		margin: 0;
		padding: 0;
		overflow: hidden;
	}

	.container {
		width: 100vw;
		height: 100vh;
		margin: 0;
		padding: 0;
		position: fixed;
		top: 0;
		left: 0;
	}

	.full-img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		/* 铺满屏幕并裁剪，不变形 ✅ */
	}
</style>