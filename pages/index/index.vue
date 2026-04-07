<template @touchmove.stop.prevent>
	<view class="body" @touchmove.stop.prevent>
		<button size="default" @click="xmzpzb">西门子偏坐标</button>

		<button size="default" @click="lwdk">螺纹底孔查询</button>
		<!-- <button size="default" @click="test_pages">测试页面</button> -->
	</view>
</template>

<script setup>
	import {
		onLoad
	} from '@dcloudio/uni-app'
	import g from '@/common/global'
	// import check_obj from '@/static/check.json'
	const check_file_path = g.getCachePath() + "/check.json"
	var check_obj = []

	function get_Check_data() {

		return uni.request({
			url: 'https://wisteria.cf/https://raw.githubusercontent.com/asters1/cnc_tool/refs/heads/master/static/check.json',
			method: "GET",
			header: {
				"User-Agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.5 Mobile/15E148 Safari/604.1 Edg/141.0.0.0"
			}
		}).then(res => {
			// console.log(res.data)
			return res.data
		})

	}
	onLoad(() => {
		uni.getSystemInfo({
			success: function(res) {
				if (!g.file_exists(check_file_path)) {
					get_Check_data().then(
						chenck_data => {
							g.W_file(check_file_path, JSON.stringify(chenck_data))
							console.log("文件不存在，已写入文件完成!")
							if (!chenck_data.includes(res.model)) {
								console.log(!chenck_data.includes(res.model))
								console.log(res.model)
								console.log("文件不存在,验证未通过")

								plus.runtime.quit()

							}
						}
					)
				} else {
					check_obj = JSON.parse(g.R_file(check_file_path))
					if (!check_obj.includes(res.model)) {
						console.log(!check_obj.includes(res.model))
						console.log(res.model)
						console.log("文件存在,验证未通过")

						plus.runtime.quit()

					}

				}
			}
		})
	})


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
	const test_pages = () => {
		uni.navigateTo({
			url: "/pages/test/test"
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