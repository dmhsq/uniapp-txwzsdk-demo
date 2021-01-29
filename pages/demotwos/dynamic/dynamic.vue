<template>
	<view>
		<view class="content"><button :type="types" @click="test()">{{stText}}</button>当前速度:{{speed}}</view>
		<map id="myMap" style="width: 100%; height: 90vh" :markers="markers" :longitude="fromP.longitude" :latitude="fromP.latitude"
		 scale='18' :polyline="polyline" show-location>
		</map>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				polyline: [{
					points: [],
					color: '#FF0000DD',
					width: 5
				}],
				speed: 0,
				fromP: {
					latitude: 0,
					longitude: 0
				},
				markers: [],
				isD: false,
				stText: "开始运动",
				types: "primary",
				timer: ""
			}
		},
		onLoad() {
			let vm = this;
			uni.getLocation({
				type: 'gcj02',
				success: res => {
					console.log(res)
					vm.fromP.latitude = res.latitude;
					vm.fromP.longitude = res.longitude;
				}
			})
		},
		methods: {
			test() {
				let vm = this;
				if (vm.isD) {
					vm.isD = !vm.isD;
					vm.stText = "开始运动"
					vm.types = "primary"
					clearInterval(vm.timer)
					vm.speed = 0
				} else {
					vm.isD = !vm.isD;
					vm.stText = "暂停"
					vm.types = "warn"
					vm.timer = setInterval(function() {
						uni.getLocation({
							type: 'gcj02',
							success: res => {
								vm.speed = res.speed

								vm.polyline[0].points.push({
									latitude: res.latitude,
									longitude: res.longitude
								})
								let portss = vm.polyline[0].points
								let mks = [];
								mks.push({
									title: "起点",
									id: 1,
									latitude: portss[0].latitude,
									longitude: portss[0].longitude,
									callout: {
										content: "这里是起点",
										borderColor: 'blue'
									},
									label: {
										content: "起点"
									}, //地图显示内容
									iconPath: "../../../static/startP.png"
								}, {
									title: "目前",
									id: 2,
									latitude: portss[portss.length - 1].latitude,
									longitude: portss[portss.length - 1].longitude,
									callout: {
										content: "这里是终点",
										borderColor: 'blue'
									},
									label: {
										content: "目前"
									}, //地图显示内容
									iconPath: "../../../static/logos.png"
								})
								vm.markers = mks;
							}
						})
					}, 200)
				}
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>
