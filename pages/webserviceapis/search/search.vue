<template>
	<view>
		<view class="mian-body">
			<uni-nav-bar :fixed="false" color="#333333" background-color="#FFFFFF" right-icon="search">
				<block slot="left" @click="onClickLeft()">
					<view class="city">
						<view><text class="uni-nav-bar-text">{{ city }}</text></view>
						<uni-icons type="arrowdown" color="#333333" size="22" style="padding-top: 12rpx;" />
					</view>
				</block>
				<view class="input-view">
					<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
					<input confirm-type="search" class="nav-bar-input" type="text" placeholder="输入搜索关键词" v-model="keys" @confirm="test()">
				</view>
			</uni-nav-bar>
		</view>
		<!-- @tap="isShow=false" -->
		<map id="myMap" :markers="markers" @tap="isShow=false" style="width:100%;height:100vh;" :longitude="longitude"
		 :latitude="latitude"  @markertap="dosome" :scale='scale' :polyline="polyline">
		</map>
		<uni-card v-if="isShow" class="cards" :title="cardData.title" :isFull="true" isShadow='true' note="额外信息" :extra="cardData.category">
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
	var QQMapWX = require('../../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				city: "周边",
				markers: [],
				latitude: 0,
				longitude: 0,
				polyline: [],
				keys: "",
				flagsO:0,
				fromP: {
					longitude: 0,
					latitude: 0
				},
				isShow: false,
				cardData: {
					title: "",
					category: "",
					address: "",
					latitude: 0,
					longitude: 0
				},
				scale: 15,
				tbData: [],
				toPs: {
					latitude: 0,
					longitude: 0
				}
			}
		},
		mounted() {
			this.getMyLocation()
		},
		onLoad() {
			this.test()
		},
		watch:{
			flagsO:function(val){
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				let vm = this
				vm.isShow = false;
				vm.markers = [];
				vm.polyline = []
				vm.keys = ""
				console.log(vm.city)
				demo.geocoder({
					address:vm.city,
					success: res=>{
						let item = res.result.location
						vm.latitude = item.lat
						vm.longitude = item.lng
					}
				})
			}
		},
		methods: {
			dosome(e) {
				let vm = this;
				console.log(e)
				let mkid = e.detail.markerId;
				let tb = vm.tbData
				console.log(tb)
				for (let i = 0; i < tb.length; i++) {
					if (parseInt(tb[i].id) == mkid) {
						console.log(tb[i])
						vm.cardData.title = tb[i].title
						vm.cardData.category = tb[i].category
						vm.cardData.address = tb[i].address
						vm.cardData.latitude = tb[i].location.lat
						vm.cardData.longitude = tb[i].location.lng
						vm.toPs.latitude = tb[i].location.lat
						vm.toPs.longitude = tb[i].location.lng
						vm.isShow = true
					}
				}
			},
			goHere() {
				console.log(123)
				let vm = this;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				demo.direction({
					//from参数不填默认当前地址
					to: vm.toPs,
					success: res => {
						console.log(res)
						let ret = res;
						let coors = ret.result.routes[0].polyline;
						let distance = ret.result.routes[0].distance;
						if (distance > 20000) {
							vm.scale = 8
						} else if (distance > 60000) {
							vm.scale = 6
						} else if (distance > 100000) {
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
						vm.latitude = toP.latitude;
						vm.longitude = toP.longitude
						mks.push({
							title: "起点",
							id: 1,
							latitude: toP.latitude,
							longitude: toP.longitude,
							callout: {
								content: "起点",
								borderColor: 'blue'
							},
							label: {
								content: "起点"
							}, //地图显示内容
							iconPath: "/static/startP.png"
						}, {
							title: "终点",
							id: 2,
							latitude: vm.toPs.latitude,
							longitude: vm.toPs.longitude,
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
			},
			test() {
				let vm = this;
				vm.polyline = [];
				vm.markers = [];
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				let cityj = ""
				if (vm.city != "周边") {
					cityj = vm.city
				}
				demo.search({
					keyword: vm.keys,
					location: `${vm.latitude},${vm.longitude}`, //位置中心
					page_size: 10, //返回条数 最大为20
					auto_extend: 0, //是否扩大选择范围 0 不扩大 1 扩大
					region: cityj,
					success: function(res) {
						console.log(res)
						let mks = []
						let dds = res.data
						vm.tbData = dds;
						dds.forEach(item => {
							let bbou = {
								points: [{
									latitude: item.location.lat,
									longitude: item.location.lng
								}],
								strokeWidth: 50,
								strokeColor: '#0055ff',
								fillColor: '#00ff7f'
							}
							if (item.boundary) {
								bbou = item.boundary
							}
							mks.push({
								title: item.title,
								id: parseInt(item.id),
								latitude: item.location.lat,
								longitude: item.location.lng,
								circle: {
									latitude: item.location.lat,
									longitude: item.location.lng,
									color: '#0055ff',
									fillColor: '#00ff7f',
									radius: 15
								},
								polygon: bbou,
								callout: {
									content: item.title,
									borderColor: 'blue'
								},
								// label:{content:item.title}, //地图显示内容
								iconPath: "../../../static/logos.png"
							})
						})
						vm.longitude = dds[0].location.lng
						vm.latitude = dds[0].location.lat
						console.log(mks)
						vm.markers = mks
					}
				})
			},
			getMyLocation() {
				let vm = this;
				uni.getLocation({
					type: 'gcj02',
					success: res => {
						vm.latitude = res.latitude
						vm.longitude = res.longitude
						vm.fromP.latitude = res.latitude
						vm.fromP.longitude = res.longitude
					}
				})
			},
			onClickLeft() {
				uni.navigateTo({
					url: '../../selectwz/selectwz'
				})
			},
			showCity() {
				uni.navigateTo({
					url: '../../selectwz/selectwz'
				})
			}
		}
	}
</script>

<style>
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

	.nav-bar-input {
		height: 30px;
		line-height: 30px;
		/* #ifdef APP-PLUS-NVUE */
		width: 370rpx;
		/* #endif */
		padding: 0 5px;
		font-size: 14px;
		background-color: #f8f8f8;
	}

	.city {
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
		margin-left: 4px;
	}

	.uni-nav-bar-text {
		font-size: 14px;
	}

	.input-view {
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		flex: 1;
		background-color: #f8f8f8;
		height: 30px;
		border-radius: 15px;
		padding: 0 15px;
		flex-wrap: nowrap;
		margin: 7px 0;
		line-height: 30px;
	}

	.input-uni-icon {
		line-height: 30px;
	}

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
