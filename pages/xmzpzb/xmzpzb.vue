<template>
	<view class="body" @touchmove.stop.prevent>
		<view class="head">
			<button @click="clear_xy">清空坐标</button>
			<button @click="history_xy">历史坐标</button>
		</view>
		<text>点1坐标</text>
		<view class="input-group">
			<!-- X1坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">X1</view>
				<input class="input-field" v-model="x1" placeholder="X1坐标" placeholder-class="input-placeholder"
					type="number" maxlength="9" />
			</view>

			<!-- Y1坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">Y1</view>
				<input class="input-field" v-model="y1" placeholder="Y1坐标" placeholder-class="input-placeholder"
					type="number" maxlength="9" />
			</view>
		</view>
		<text>点2坐标</text>
		<view class="input-group">
			<!-- X2坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">X2</view>
				<input class="input-field" v-model="x2" placeholder="X2坐标" placeholder-class="input-placeholder"
					type="number" maxlength="9" />
			</view>

			<!-- Y2坐标输入框 -->
			<view class="input-item">
				<view class="input-prefix">Y2</view>
				<input class="input-field" v-model="y2" placeholder="Y2坐标" placeholder-class="input-placeholder"
					type="number" maxlength="9" />
			</view>
		</view>
		<button @click="run_xy">计算角度</button>
		<view class="result">
			<view>{{text_deg}}</view>
			<view>{{text_len}}</view>
		</view>

	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'
	import g from '@/common/global'
	const x1 = ref(null)
	const x2 = ref(null)
	const y1 = ref(null)
	const y2 = ref(null)

	const deg = ref(null)
	const len = ref(null)

	const text_deg = ref("请输入坐标！！！")
	const text_len = ref("")
	var history_file_path = ""
	// console.log("aaa")


	history_file_path = g.getCachePath() + "/history.json"
	if (g.file_exists(history_file_path)) {
		// console.log("存在")
		try {
			g.history_data = JSON.parse(g.R_file(history_file_path))
		} catch {
			g.W_file(history_file_path, JSON.stringify(g.history_data))
		}

	} else {
		g.W_file(history_file_path, JSON.stringify(g.history_data))

	}

	function GetLineAngle(dx, dy) {
		if (dx === 0) {
			return 90
		}
		if (dy === 0) {
			return 0
		}
		const rad = Math.atan(dy / dx)
		const deg = rad * 180 / Math.PI
		return deg
	}
	const run_xy = () => {
		const dx = parseFloat(x1.value) - parseFloat(x2.value)
		const dy = parseFloat(y1.value) - parseFloat(y2.value)
		deg.value = GetLineAngle(dx, dy)
		len.value = Math.sqrt(dx * dx + dy * dy)
		if (!isNaN(deg.value)) {

			deg.value = deg.value.toFixed(3)
			len.value = len.value.toFixed(3)
			if (deg.value == 0) {
				text_deg.value = "X1和X2处于同一水平线上，不需要偏置坐标！"
			} else {
				text_deg.value = "Z轴旋转角度为" + deg.value + "°"

			}
			text_len.value = "长度为: " + len.value
			var data_obj = {}
			data_obj.x1 = x1.value
			data_obj.x2 = x2.value
			data_obj.y1 = y1.value
			data_obj.y2 = y2.value
			data_obj.deg = text_deg.value
			data_obj.len = text_len.value
			data_obj.time = g.Get_Date_Name()
			// console.log(JSON.stringify(data_obj))
			if (g.history_data.length > 20) {
			g.history_data.shift()
				
			}
			g.history_data.push(data_obj)
			g.W_file(history_file_path, JSON.stringify(g.history_data))
			console.log(g.history_data.length)
		}

	}
	const history_xy=()=>{
		console.log("历史坐标")
		
	}
</script>

<style>
	.body {
		margin-top: 30px;
		display: flex;
		flex-direction: column;
		/* justify-content: space-around; */
		align-items: center;
		font-size: 30px;
		height: 100%;

	}

	.head {
		display: flex;
		flex-direction: row;
		width: 70%;

	}

	.result {
		margin-left: 30px;
		margin-top: 30px;
		display: flex;
		flex-direction: column;
		font-size: 18px;
		justify-content: flex-start;
		/* align-items: flex-start; */
		width: 100%;
	}

	text {
		margin-top: 30px;
	}

	button {
		width: 50%;
		margin-top: 40px;
		background-color: #ccc;
		color: #000;

	}

	.head button {
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