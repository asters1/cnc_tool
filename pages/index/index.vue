<template @touchmove.stop.prevent>
	<view class="body" @touchmove.stop.prevent>
		<button size="default" @click="xmzpzb">西门子偏坐标</button>
		<button size="default" @click="lwdk">螺纹底孔查询</button>
	</view>
</template>

<script setup>
	const xmzpzb = () => {
		uni.navigateTo({
			url: "/pages/xmzpzb/xmzpzb"
		})
	}
	const lwdk = () => {
		uni.navigateTo({
			url: "/pages/lwdk/lwdk"
		})
	}
</script>

<style>
	.body {
		margin-top: 70px;
		height: 100%;
	}

	button {
		width: 70%;
		margin-top: 30px;
		background-color: #ccc;
		color: #000;
	}
</style>