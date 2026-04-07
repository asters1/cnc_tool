<template @touchmove.stop.prevent>
	<view class="body" @touchmove.stop.prevent>
		<view class="head">


			<view class="input-item">
				<view class="input-prefix">M</view>
				<input class="input-field" v-model="m_num" placeholder="10" type="number" maxlength="9" />
			</view>
			<button @click="query_M">查询</button>
		</view>
		<view class="result">
			<view class="text_result">
				公制粗牙:&nbsp;&nbsp;&nbsp;&nbsp;{{cuya_txt}}
			</view>
			<view class="text_result">
				公制细牙:&nbsp;&nbsp;&nbsp;&nbsp;{{xiya_txt}}
			</view>
		</view>
		<view class="img_btn_view">
			<button class="img_btn" @click="img_btn(1)">粗牙表</button>
			<button class="img_btn" @click="img_btn(2)">细牙表</button>
		</view>
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'

	const m_num = ref(null)

	const cuya_txt = ref(null)
	const xiya_txt = ref(null)


	const C_JSON = {
		"1": ["M1X0.25 (0.75)"],
		"1.1": ["M1.1X0.25 (0.85)"],
		"1.2": ["M1.2X0.25 (0.95)"],
		"1.4": ["M1.4X0.3 (1.1)"],
		"1.6": ["M1.6X0.35 (1.25)"],
		"1.7": ["M1.7X0.35 (1.35)"],
		"1.8": ["M1.8X0.35 (1.45)"],
		"2": ["M2X0.4 (1.6)"],
		"2.2": ["M2.2X0.45 (1.75)"],
		"2.3": ["M2.3X0.4 (1.9)"],
		"2.5": ["M2.5X0.45 (2.1)"],
		"2.6": ["M2.6X0.45 (2.2)"],
		"3": ["M3X0.5 (2.5)"],
		"3.5": ["M3.5X0.6 (2.9)"],
		"4": ["M4X0.7 (3.3)"],
		"4.5": ["M4.5X0.75 (3.8)"],
		"5": ["M5X0.8 (4.2)"],
		"6": ["M6X1 (5)"],
		"7": ["M7X1 (6)"],
		"8": ["M8X1.25 (6.8)"],
		"9": ["M9X1.25 (7.8)"],
		"10": ["M10X1.5 (8.5)"],
		"11": ["M11X1.5 (9.5)"],
		"12": ["M12X1.75 (10.3)"],
		"14": ["M14X2 (12)"],
		"16": ["M16X2 (14)"],
		"18": ["M18X2.5 (15.5)"],
		"20": ["M20X2.5 (17.5)"],
		"22": ["M22X2.5 (19.5)"],
		"24": ["M24X3 (21)"],
		"27": ["M27X3 (24)"],
		"30": ["M30X3.5 (26.5)"],
		"33": ["M33X3.5 (29.5)"],
		"36": ["M36X4 (32)"],
		"39": ["M39X4 (35)"],
		"42": ["M42X4.5 (37.5)"],
		"45": ["M45X4.5 (40.5)"],
		"48": ["M48X5 (43)"],
		"52": ["M52X5 (47)"],
		"56": ["M56X5.5 (50.5)"],
		"60": ["M60X5.5 (54.5)"],
		"64": ["M64X6 (58)"],
		"68": ["M68X6 (62)"]
	}
	const X_JSON = {
		"8": ["M8X1(6.92)"],
		"10": ["M10X1(8.92)", "M10X1.25(8.65)"],
		"12": ["M12X1.25(10.7)", "M12X1.5(10.4)"],
		"14": ["M14X1.5(12.4)"],
		"16": ["M16X1.5(14.4)"],
		"18": ["M18X1.5(16.4)", "M18X2(15.8)"],
		"20": ["M20X1.5(18.4)"],
		"22": ["M22X1.5(20.4)", "M22X2(19.8)"],
		"24": ["M24X2(21.8)"],
		"27": ["M27X2(24.8)"],
		"30": ["M30X2(27.8)"],
		"33": ["M33X2(30.8)"],
		"36": ["M36X3(32.8)"],
		"39": ["M39X3(35.8)"],
		"42": ["M42X3(38.8)"],
		"45": ["M45X3(41.8)"],
		"48": ["M48X3(44.8)"],
		"52": ["M52X4(47.7)"],
		"56": ["M56X4(51.7)"],
		"60": ["M60X4(55.7)"],
		"64": ["M64X4(59.7)"]
	}
	const img_btn=(index)=>{
		if(index==1){
			uni.navigateTo({
				url: "/pages/img/img?img_path=GZCY"
			})
		}else{
			uni.navigateTo({
				url: "/pages/img/img?img_path=GZXY"
			})
		}
	}
	const query_M = () => {
		xiya_txt.value = ""
		cuya_txt.value = ""
		if (m_num.value == null) {
			try {


				cuya_txt.value = C_JSON["10"].join(",")
			}catch{}
			try {
				xiya_txt.value = X_JSON["10"].join(",")
			}catch{}


		} else {
			try {
				cuya_txt.value = C_JSON[m_num.value].join(",")
			}catch{}
			try {
				xiya_txt.value = X_JSON[m_num.value].join(",")
			}catch{}
		}
	}
</script>

<style>
	.img_btn_view {
		display: flex;
		flex-direction: row;

	}

	.result {
		padding-top: 30px;
	}

	.text_result {
		margin-top: 40px;
		/* margin-left: 30px; */
		font-size: 18px;
	}

	.body {
		height: 100%;
	}

	.head {
		margin-top: 100px;
		display: flex;
	}

	button {
		width: 20%;
		margin-left: 140px;
		margin-right: 40px;
		background-color: #ccc;
		color: #000;
	}

	.img_btn {
		width: 40%;
		margin-top: 60px;
		margin-left: 40px;
		margin-right: 40px;
		background-color: #ccc;
		color: #000;
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
		margin-left: 40px;
		align-items: center;
		border-radius: 10px;
		background: linear-gradient(135deg, #fff 0%, #e0e7f3 100%);
		box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.1);
		padding: 24rpx 32rpx;
		box-sizing: border-box;
		width: 20%;
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
</style>