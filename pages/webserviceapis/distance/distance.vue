<template>
	<view>
		<view class="content">
			起点:<input class="uni-input" placeholder="起点,输入格式 33.404659,115.089099,不填为当前位置" style="border: 1px solid #0055FF;" v-model="fromP">
			终点:<input class="uni-input" placeholder="终点,输入格式 33.404659,115.089099" style="border: 1px solid #0055FF;" v-model="toP">
			距离:<input class="uni-input" placeholder="距离" disabled style="border: 1px solid #0055FF;" v-model="dis">
			<button type="primary" @click="test()">点击计算</button>
		</view>
		<map id="myMap" style="width: 100%; height: 70vh" :markers="markers" :longitude="longitude" :latitude="latitude"
		 :scale='scale' :polyline="polyline" show-location>
		</map>
		<view class="main-body">
			地图
		</view>
	</view>
</template>

<script>
	var QQMapWX = require('../../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				fromP: '33.403211,115.086036',
				toP: '33.396996,115.080716',
				longitude: 115.086036,
				latitude: 33.403211,
				polyline: [],
				markers: [],
				type: "",
				scale:13,
				dis:0
			}
		},
		methods: {
			getType(e){
				this.type = e.detail.value;
			},
			test() {
				let vm = this;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				//只测试距离使用 calculateDistance
				demo.direction({
					mode: vm.type, //可选值：'driving'（驾车）、'walking'（步行）、'bicycling'（骑行），不填默认：'driving',可不填
					//from参数不填默认当前地址
					from: vm.fromP,
					to: vm.toP,
					success: res => {
						console.log(res)
						let ret = res;
						let coors = ret.result.routes[0].polyline;
						let distance = ret.result.routes[0].distance;
						if(distance>1000){
							vm.dis = (distance/1000).toString().substr(0,3)+"km"
						}else{
							vm.dis = distance+"m"
						}
						
						if(distance>20000){
							vm.scale = 8
						}else if(distance>60000){
							vm.scale = 6
						}else if(distance>100000){
							vm.scale = 4
						}
						let pl = [];
						let kr = 1000000;
						//坐标解压（返回的点串坐标，通过前向差分进行压缩）
						for (var i = 2; i < coors.length; i++) {
							coors[i] = Number(coors[i - 2]) + Number(coors[i]) / kr;
						}
						//将解压后的坐标放入点串数组pl中
						for (var i = 0; i < coors.length; i += 2) {
							pl.push({
								latitude: coors[i],
								longitude: coors[i + 1]
							})
						}
						vm.latitude = pl[0].latitude
						vm.longitude = pl[0].longitude
						vm.polyline = [{
							points: pl,
							color: '#FF0000DD',
							width: 4
						}]
						let tops = pl[pl.length-1]
						let mks = []
						mks.push({
							title: "起点",
							id: 1,
							latitude: vm.latitude,
							longitude: vm.longitude,
							callout: {
								content: "起点",
								borderColor: 'blue'
							},
							label: {
								content: "起点"
							},
							iconPath: "../../../static/startP.png"
						}, {
							title: "终点",
							id: 2,
							latitude: tops.latitude,
							longitude: tops.longitude,
							callout: {
								content: "终点",
								borderColor: 'blue'
							},
							label: {
								content: "终点"
							},
							iconPath: "../../../static/endP.png"
						})
						vm.markers = mks
					}
				})
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
		background-color: #C0C0C0;
	}

	.main-body {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		padding: 0;
		font-size: 14px;
		background-color: #ffffff;
	}

	.uni-input {
		height: 28px;
		line-height: 28px;
		font-size: 15px;
		padding: 0px;
		flex: 1;
		background-color: #FFFFFF;
	}
</style>
