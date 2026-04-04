<template>
	<view class="body" @touchmove.stop.prevent>
		<view class="head" >
		<button @click="clear_xy">清空坐标</button>
		<button @click="history_xy">历史坐标</button>
		</view>
		<text>点1坐标</text>
		<view class="input-group">
			<!-- X1坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">X1</view>
				<input class="input-field" v-model="x1" placeholder="X1坐标" placeholder-class="input-placeholder"
					type="number" />
			</view>

			<!-- Y1坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">Y1</view>
				<input class="input-field" v-model="y1" placeholder="Y1坐标" placeholder-class="input-placeholder"
					type="number" />
			</view>
		</view>
		<text>点2坐标</text>
		<view class="input-group">
			<!-- X1坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">X2</view>
				<input class="input-field" v-model="x2" placeholder="X2坐标" placeholder-class="input-placeholder"
					type="number" />
			</view>
		
			<!-- Y1坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">Y2</view>
				<input class="input-field" v-model="y2" placeholder="Y2坐标" placeholder-class="input-placeholder"
					type="number" />
			</view>
		</view>
		<button @click="run_xy">计算角度</button>
	</view>
</template>

<script setup>
</script>

<style>
	.body {
		margin-top: 30px;
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
		font-size: 30px;
		/* height: 50%; */
		
	}
	.head{
		display: flex;
		flex-direction: row;
		width: 70%;
		
	}

	text {
		margin-top: 30px;
	}

	button {
		width: 50%;
		margin-top: 20px;
		background-color: #ccc;
		color: #000;
	
	}
	.head button{
			margin-left: 30px;
	}

	.input-group {
		display: flex;
		margin-top: 10px;
		gap: 20rpx;
		/* 两个输入框之间的间距 */
		padding: 20rpx;
	}

	.input-item {
		flex: 1;
		/* 两个输入框平分宽度 */
		display: flex;
		margin-left: 18px;
		align-items: center;
		/* 圆角+渐变背景+阴影，还原设计效果 */
		border-radius: 10px;
		background: linear-gradient(135deg, #ffffff 0%, #e0e7f3 100%);
		box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.1);
		padding: 24rpx 32rpx;
		box-sizing: border-box;
	}

	.input-prefix {
		/* 左侧X1/Y1标签样式 */
		font-size: 18px;
		/* font-weight: bold; */
		color: #000000;
		margin-right: 24rpx;
		line-height: 1;
		flex-shrink: 0;
		/* 禁止压缩 */
	}

	.input-field {
		/* 右侧输入框样式 */
		flex: 1;
		font-size: 18px;
		color: #000;
		background: transparent;
		/* 透明背景，继承外层渐变 */
		border: none;
		outline: none;
	}

	/* 占位符样式 */
	.input-placeholder {
		color: #999999;
		font-size: 20px;
	}
</style>