<template>
	<view>
		<map id="myMap" style="width: 100%; height: 100vh" :markers="markers" latitude="36.315125" longitude="104.238281" scale='8' show-location>
		</map>
	</view>
</template>

<script>
	var QQMapWX = require('../../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				markers: []
			}
		},
		onLoad() {
			this.test()
		},
		methods: {
			test() {
				
				let vm = this;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				demo.getCityList({
					success: res => {
						console.log(res.result)
						let cityOne = res.result[0]
						let cityTwo = res.result[1]
						// let cityThr = res.result[2]
						let mks = [];
						
						cityTwo.forEach(item => {
							mks.push({
								title: item.name,
								id: parseInt(item.id),
								latitude: item.location.lat,
								longitude: item.location.lng,
								iconPath: "../../../static/shi.png"
							})
						})
						cityOne.forEach(item => {
							mks.push({
								title: item.name,
								id: parseInt(item.id),
								latitude: item.location.lat,
								longitude: item.location.lng,
								iconPath: "../../../static/sheng.png"
							})
						}) 
						uni.showToast({
							title:'城市过多可能加载慢',
							icon:'none',
							duration:4000
						})
						vm.markers = mks;
					}
				})
			}
		}
	}
</script>

<style>

</style>
