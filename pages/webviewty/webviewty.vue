<template>
	<view>
		<view style="width: 100%;height: 100vh;">
			<map id="myMap" :markers="markers" style="width:100%;height:100vh;z-index: 1;" :longitude="fromP.longitude"
			 :latitude="fromP.latitude" :scale='scale' :polyline="polyline" show-location>
			</map>
		</view>
		<uni-card class="cards" :title="cardData.title" :isFull="true" isShadow='true' note="额外信息" :extra="cardData.category" >
			<text class="content-box-text">{{cardData.address}}</text>
			<block slot="footer">
				<view class="footer-box">
					<!-- <view class="" @click.stop="footerClick('喜欢')"><text class="footer-box__item">喜欢</text></view>
					<view class="" @click.stop="footerClick('评论')"><text class="footer-box__item">评论</text></view> -->
					<view><text class="footer-box__item" @click="goHere()">去这里</text></view>
				</view>
			</block>
		</uni-card>
	</view>
</template>

<script>
	var QQMapWX = require('../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				markers: [],
				fromP: {
					longitude: 0,
					latitude: 0
				},
				cardData:{
					title:"",
					category:"",
					address:""
				},
				toPs:{
					longitude: 0,
					latitude: 0
				},
				polyline:[],
				scale:12
			}
		},
		onLoad(opt) {
			
			let item = JSON.parse(opt.item)
			console.log(item)
			this.toPs.latitude = item.location.lat;
			this.toPs.longitude = item.location.lng;
			this.fromP = this.toPs
			uni.setNavigationBarTitle({
			    title: item.title
			});
			this.cardData.title = item.title;
			this.cardData.category = item.category
			this.cardData.address = item.address
			let mks = [];
			mks.push({
				title: item.title,
				id: parseInt(item.id),
				latitude: item.location.lat,
				longitude: item.location.lng,
				callout: {
					content: item.title,
					borderColor: 'blue'
				},
				// label:{content:item.title}, //地图显示内容
				iconPath: "../../static/logos.png"
			})
			this.markers = mks
			
		},
		methods: {
			goHere() {
				console.log(123)
				let vm = this;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				demo.direction({
					to: vm.toPs,
					success: res => {
						console.log(res)
						let ret = res;
						let coors = ret.result.routes[0].polyline;
						let distance = ret.result.routes[0].distance;
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
						let toP = vm.fromP;
						vm.polyline = [{
							points: pl,
							color: '#FF0000DD',
							width: 4
						}]
						let mks = []
						console.log(toP)
						console.log(vm.toPs)
						mks.push({
							title: "终点",
							id: 1,
							latitude: toP.latitude,
							longitude: toP.longitude,
							callout: {
								content: "终点",
								borderColor: 'blue'
							},
							label: {
								content: "终点"
							}, //地图显示内容
							iconPath: "/static/endP.png"
						})
						vm.markers = [];
						vm.markers = mks
					}
				})
			}
		}
	}
</script>

<style>
	.cards {
		width: 80%;
		padding-left: 10%;
		position: fixed;
		bottom: 0;
		z-index: 99;
	}

	.content-box-text {
		font-size: 12px;
		line-height: 22px;
	}

	.footer-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		justify-content: space-between;
		flex-direction: row;
	}

	.right-footer {
		float: right;
	}

	.footer-box__item {
		align-items: center;
		padding: 2px 0;
		font-size: 12px;
		color: #666;
	}
</style>
