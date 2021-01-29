<template>
	<view>
		<map id="myMap" style="width: 100%; height: 90vh" :markers="markers" :longitude="longitude" :latitude="latitude"
		 scale='18' :polyline="polyline" show-location>
		</map>
	</view>
</template>

<script>
	var QQMapWX = require('../../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				fromP: '33.403977,115.088245',
				toP: '33.404659,115.089099',
				longitude: 115.088245,
				latitude: 33.403977,
				polyline: [],
				markers: []
			}
		},
		onLoad() {
			this.test();
		},
		methods: {
			test() {
				let vm = this;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				demo.direction({
					mode: 'walking', //可选值：'driving'（驾车）、'walking'（步行）、'bicycling'（骑行），不填默认：'driving',可不填
					//from参数不填默认当前地址
					from: vm.fromP,
					to: vm.toP,
					success: res => {
						console.log(res)
						let ret = res;
						let coors = ret.result.routes[0].polyline;
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
						let mks = []
						mks.push({
							title: "主要",
							id: 11,

							latitude: 33.404659,
							longitude: 115.089099,
							label: {
								content: "主要",
								color: "#0000ff"
							},
							callout: {
								content: "主要",
								borderColor: 'blue'
							},
							iconPath: "../../../static/mainPeo.png"
						}, {
							title: "普通1",
							id: 112,

							latitude: 33.403977,
							longitude: 115.088245,
							label: {
								content: "普通1",
								color: "#000000"
							},
							callout: {
								content: "普通1",
								borderColor: 'blue'
							},
							iconPath: "../../../static/somePeo.png"
						})
						vm.markers = mks
					}
				})
			}
		}
	}
</script>
