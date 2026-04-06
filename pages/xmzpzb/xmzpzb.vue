<template>
	<view class="body">
		<view class="head">
			<button @click="clear_xy">清空坐标</button>
			<button @click="showModal = true">历史坐标</button>
		</view>

		<!-- 弹窗 -->
		<view v-if="showModal" class="mask" @click="showModal = false">
			<view class="panel" @click.stop>
				<scroll-view scroll-y class="list-scroll" enhanced="true">
					<view v-for="(item, idx) in g.history_data.slice().reverse()" :key="idx" class="item" @click="select(idx, item)">
						{{idx+1}}.{{`   `+item.time }}
					</view>
				</scroll-view>
				<view class="cancel" @click="showModal = false">取消</view>
			</view>
		</view>

		<text>点1坐标</text>
		<view class="input-group">
			<view class="input-item">
				<view class="input-prefix">X1</view>
				<input class="input-field" v-model="x1" placeholder="X1坐标" type="number" maxlength="9" />
			</view>
			<view class="input-item">
				<view class="input-prefix">Y1</view>
				<input class="input-field" v-model="y1" placeholder="Y1坐标" type="number" maxlength="9" />
			</view>
		</view>

		<text>点2坐标</text>
		<view class="input-group">
			<view class="input-item">
				<view class="input-prefix">X2</view>
				<input class="input-field" v-model="x2" placeholder="X2坐标" type="number" maxlength="9" />
			</view>
			<view class="input-item">
				<view class="input-prefix">Y2</view>
				<input class="input-field" v-model="y2" placeholder="Y2坐标" type="number" maxlength="9" />
			</view>
		</view>

		<button @click="run_xy">计算角度</button>
		<view class="result">
			<view>{{ text_deg }}</view>
			<view>{{ text_len }}</view>
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
	const text_deg = ref("请输入坐标！！！")
	const text_len = ref("")
	const showModal = ref(false)

	const history_file_path = g.getCachePath() + "/history.json"
	if (g.file_exists(history_file_path)) {
		try {
			g.history_data = JSON.parse(g.R_file(history_file_path))
		} catch {
			g.W_file(history_file_path, JSON.stringify([]))
		}
	} else {
		g.W_file(history_file_path, JSON.stringify([]))
	}

	function GetLineAngle(dx, dy) {
		if (dx === 0) return 90
		if (dy === 0) return 0
		return Math.atan(dy / dx) * 180 / Math.PI
	}

	const run_xy = () => {
		const dx = parseFloat(x1.value) - parseFloat(x2.value)
		const dy = parseFloat(y1.value) - parseFloat(y2.value)
		const deg = GetLineAngle(dx, dy)
		const len = Math.sqrt(dx * dx + dy * dy)

		if (!isNaN(deg)) {
			text_deg.value = deg === 0 ? "同一水平线" : `角度：${deg.toFixed(3)}°`
			text_len.value = `长度：${len.toFixed(3)}`

			const data_obj = {
				x1: x1.value,
				y1: y1.value,
				x2: x2.value,
				y2: y2.value,
				deg: text_deg.value,
				len: text_len.value,
				time: g.Get_Date_Name()
			}
			if (g.history_data.length >= 20) g.history_data.shift()
			g.history_data.push(data_obj)
			g.W_file(history_file_path, JSON.stringify(g.history_data))
		}
	}

	function clear_xy() {
		x1.value = x2.value = y1.value = y2.value = null
		text_deg.value = "请输入坐标！！！"
		text_len.value = ""
	}

	function select(index, item) {
		x1.value = item.x1
		y1.value = item.y1
		x2.value = item.x2
		y2.value = item.y2
		text_deg.value = item.deg
		text_len.value = item.len
		showModal.value = false
	}
</script>

<style scoped>
	/* 关键：用 CSS 固定页面，不再用 touchmove.prevent */
	.body {
		margin-top: 30px;
		display: flex;
		flex-direction: column;
		align-items: center;
		font-size: 30px;
		height: 100vh;
		/* 占满屏幕 */
		overflow: hidden;
		/* 页面本身禁止滚动 */
		box-sizing: border-box;
	}

	.head {
		display: flex;
		width: 70%;
	}

	.result {
		margin-left: 30px;
		margin-top: 30px;
		font-size: 18px;
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
		padding: 20rpx;
	}

	.input-item {
		flex: 1;
		display: flex;
		margin-left: 18px;
		align-items: center;
		border-radius: 10px;
		background: linear-gradient(135deg, #fff 0%, #e0e7f3 100%);
		box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.1);
		padding: 24rpx 32rpx;
		box-sizing: border-box;
	}

	.input-prefix {
		font-size: 18px;
		margin-right: 24rpx;
	}

	.input-field {
		flex: 1;
		font-size: 18px;
		background: transparent;
		border: none;
		outline: none;
	}

	/* 弹窗 */
	.mask {
		position: fixed;
		inset: 0;
		background: rgba(0, 0, 0, 0.5);
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 999;
		font-size: 18px;
	}

	.panel {
		width: 88%;
		background: #fff;
		border-radius: 20rpx;
		overflow: hidden;
	}

	.list-scroll {
		max-height: 60vh;
	}

	.item {
		padding: 24rpx;
		text-align: center;
		border-bottom: 1rpx solid #eee;
		/* font-size: 10px; */
	}

	.cancel {
		padding: 24rpx;
		text-align: center;
		background: #f8f8f8;
		font-weight: 500;
	}
</style>