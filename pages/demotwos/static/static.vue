<template>
	<view>
		<map id="myMap" style="width: 100%; height: 100vh" :markers="markers" :longitude="fromP.longitude" :latitude="fromP.latitude"
		 scale='18' :polyline="polyline" show-location>
		</map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				fromP: {
					longitude: 115.101399,
					latitude: 33.415668
				},
				endP: {
					longitude: 115.101399,
					latitude: 33.415668
				},
				polyline: [],
				markers: []
			}
		},
		onLoad() {
			this.test()
		},
		methods: {
			test() {
				let vm = this;
				//模拟实时获取位置
				let ports = [{
					latitude: 33.415668,
					longitude: 115.101399
				}, {
					latitude: 33.415834,
					longitude: 115.101603
				}, {
					latitude: 33.415811,
					longitude: 115.101877
				}, {
					latitude: 33.415905,
					longitude: 115.102370
				}, {
					latitude: 33.416389,
					longitude: 115.101802
				}, {
					latitude: 33.416232,
					longitude: 115.101603
				}, {
					latitude: 33.416196,
					longitude: 115.101448
				}, {
					latitude: 33.416192,
					longitude: 115.101383
				}, {
					latitude: 33.416277,
					longitude: 115.101265
				}, {
					latitude: 33.416026,
					longitude: 115.101115
				}, {
					latitude: 33.415941,
					longitude: 115.100804
				}];
				let lasts = ports.length;
				let startS = ports[0]
				let endD = ports[lasts - 1]
				vm.endP = endD
				let mks = []
				console.log(ports)
				mks.push({
					title: "起点",
					id: 1,
					latitude: startS.latitude,
					longitude: startS.longitude,
					callout: {
						content: "这里是起点",
						borderColor: 'blue'
					},
					label: {
						content: "起点"
					}, //地图显示内容
					iconPath: "../../../static/startP.png"
				}, {
					title: "终点",
					id: 2,
					latitude: endD.latitude,
					longitude: endD.longitude,
					callout: {
						content: "这里是终点",
						borderColor: 'blue'
					},
					label: {
						content: "终点"
					}, //地图显示内容
					iconPath: "../../../static/endP.png"
				})
				vm.markers = mks;
				let colors = [];

				function getRandomColor() {
					const rgb = []
					for (let i = 0; i < 3; ++i) {
						let color = Math.floor(Math.random() * 256).toString(16)
						color = color.length == 1 ? '0' + color : color
						rgb.push(color)
					}
					return '#' + rgb.join('')
				}
				for (let j = 0; j < ports.length; j++) {
					let stro = getRandomColor()
					colors.push(stro)
				}
				let colorLists = colors
				console.log(colorLists)
				// Math.floor(Math.random()*10); 
				// var ranNum = Math.ceil(Math.random() * 25);

				vm.polyline = [{
					points: ports,
					// color: '#FF0000DD',
					//这里可以随机生成
					colorList: colors, // ['#ffff00','#0f1bff','#ffc859','#ff1770','#ff0000','#e28aff','#00aa00','#55ff7f','#0651ff','#000000',]
					arrowLine: true,
					width: 5,
					borderColor: "#ffff00"
				}]
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
