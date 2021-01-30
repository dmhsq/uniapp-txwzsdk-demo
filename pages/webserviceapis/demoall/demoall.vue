<template>
	<view>
		<view v-if="!isDH" class="mian-body" style="z-index: 120;position: fixed;left: 0;top: 0;">
			<uni-nav-bar :fixed="false" color="#333333" background-color="#FFFFFF" >
				<block slot="left" @click="onClickLeft()">
					<view class="city">
						<view><text class="uni-nav-bar-text">{{ city }}</text></view>
						<uni-icons type="arrowdown" color="#333333" size="22" style="padding-top: 12rpx;" />
					</view>
				</block>
				<view class="input-view">
					<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
					<input confirm-type="search" class="nav-bar-input" type="text" placeholder="输入搜索关键词" v-model="keys" @focus="search()"
					 @input="search()" @confirm="search()">
				</view>
			</uni-nav-bar>
		</view>
		<view v-if="isDH" style="z-index: 50;">
			<view class="mian-body">
				<uni-nav-bar :fixed="false" color="#333333" background-color="#FFFFFF" right-icon="search">
					<block slot="left" @click="onClickLeft()">
						<view class="city">
							<view><text class="uni-nav-bar-text">起点</text></view>
							<uni-icons type="arrowdown" color="#333333" size="22" style="padding-top: 12rpx;" />
						</view>
					</block>
					<view class="input-view">
						<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
						<input confirm-type="search" class="nav-bar-input" type="text" placeholder="输入搜索关键词" v-model="keys" @focus="search()"
						 @input="search()" @confirm="search()">
					</view>
				</uni-nav-bar>
			</view>
			<view class="mian-body">
				<uni-nav-bar :fixed="false" color="#333333" background-color="#FFFFFF" right-icon="search">
					<block slot="left" @click="onClickLeft()">
						<view class="city">
							<view><text class="uni-nav-bar-text">终点</text></view>
							<uni-icons type="arrowdown" color="#333333" size="22" style="padding-top: 12rpx;" />
						</view>
					</block>
					<view class="input-view">
						<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
						<input confirm-type="search" class="nav-bar-input" type="text" placeholder="输入搜索关键词" v-model="keys" @focus="search()"
						 @input="search()" @confirm="search()">
					</view>
				</uni-nav-bar>
			</view>
		</view>
		<!-- @tap="isShow=false" -->
		<map id="myMap" @regionchange="changeScale" :markers="markers" @tap="isMap" style="width:100%;height:100vh;" :longitude="longitude" :latitude="latitude"
		 @markertap="dosome" :scale='scale' :polyline="polyline">
		</map>
		<view style="position: fixed;top: 15%;right: 15%;width: 300rpx;height: 50rpx;">
			<image @click="changSize('down')" src="../../../static/jianh.png" style="width: 30rpx;height: 30rpx;float: left;position: relative;top: 20rpx;"></image>
			<slider style="width: 200rpx;padding-left: 15rpx;" :value="scale" @change="sliderChange" @changing="sliderChange" min="3" max="20" activeColor="#006d00" backgroundColor="#000000"
			 block-color="#8A6DE9" block-size="15" />
			<image @click="changSize('up')" src="../../../static/jiah.png" style="width: 30rpx;height: 30rpx;position: absolute;right: 0;top: 20rpx;"></image>
		</view>
		<scroll-view scroll-y="true" v-if="isList" style="position: absolute;width: 80%;left: 10% ;height: 1000rpx;top:100rpx;background-color: #ffffff;z-index: 100;" :class="{dh:isDH}">
			<uni-list>
				<uni-list-item v-for="item in tables" :title="item.title" :note="item.address" showArrow clickable="" :rightText="item.category"
				 @click="goMap(item)" />
			</uni-list>
		</scroll-view>
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
		<image src="../../../static/goDH.png" class="dh-img" @click="goDH()"></image>
	</view>
</template>

<script>
	var QQMapWX = require('../../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				city: "未知",
				markers: [],
				flagsO:0,
				flagsL:0,
				latitude: 0,
				isDH: false,
				longitude: 0,
				polyline: [],
				keys: "",
				isShow: false,
				cardData: {
					title: "",
					category: "",
					address: "",
					latitude: 0,
					longitude: 0
				},
				fromP: {
					longitude: 0,
					latitude: 0
				},
				scale: 15,
				tbData: [],
				isList: false,
				toPs: {
					latitude: 0,
					longitude: 0
				},
				tables: []
			}
		},
		mounted() {
			this.getMyLocation()
		},
		watch:{
			flagsO:function(val){
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				let vm = this
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
		onLoad() {
			
			// this.test()
		},
		methods: {
			showSize(){
				uni.showToast({
					title:'缩放等级'+this.scale,
					icon:'none',
					duration:1500
				})
			},
			changSize(type) {
				if(this.scale==1&&type=='down'){
					uni.showToast({
						title:'已缩放至最小',
						icon:'none',
						duration:1500
					})
					return ;
				}
				if(this.scale==20&&type=='up'){
					uni.showToast({
						title:'已缩放至最大',
						icon:'none',
						duration:1500
					})
					return ;
				}
				if(type=='up'){
					this.scale++;
					this.showSize()
					return ;
				}
				if(type=='down'){
					this.scale--;
					this.showSize()
					return ;
				}
			},
			sliderChange(e) {
				this.scale = e.detail.value
				this.showSize()
			},
			changeScale(e){
				console.log(e)
				if(e.causedBy =='update'){
					return ;
				}
				console.log(this.scale)
				if(e.type == 'end'){
					if(this.scale==3){
						uni.showToast({
							title:'已缩放至最小',
							icon:'none',
							duration:1500
						})
					}
					if(this.scale==20){
						uni.showToast({
							title:'已缩放至最大',
							icon:'none',
							duration:1500
						})
					}
					this.scale = e.detail.scale
				}
				
			},
			goDH(){
				uni.showToast({
					title:'导航服务此版本不整合',
					icon:'none',
					duration:2000
				})
				return ;
				let vm = this;
				vm.isDH=true
				vm.isShow = false
				vm.isList = false
				vm.markers = []
				vm.polyline = []
			},
			goMap(item) {
				let vm = this;
				vm.polyline = [];
				vm.cardData.title = item.title
				vm.cardData.category = item.category
				vm.cardData.address = item.address
				vm.cardData.latitude = item.location.lat
				vm.cardData.longitude = item.location.lng
				vm.isList = false;
				vm.isShow = true;
				vm.toPs.latitude = item.location.lat;
				vm.toPs.longitude = item.location.lng;
				vm.latitude = item.location.lat
				vm.longitude = item.location.lng
				let mks = []
				mks.push({
					title: item.title,
					id: 1,
					latitude: item.location.lat,
					longitude: item.location.lng,
					callout: {
						content: item.title,
						borderColor: 'blue'
					},
					label: {
						content: item.title
					}, //地图显示内容
					iconPath: "/static/logos.png"
				})
				vm.markers = mks
			},
			isMap(e) {
				let vm = this;
				
				if (vm.isShow) {
					vm.isShow = false
				}else if(vm.isList){
					vm.isList = false
					vm.isShow = false
				}
				 else if(vm.isDH){
					vm.isShow = false
					vm.isList = false
					vm.isDH = false
				} else {
					vm.polyline = [];
					vm.markers = [];
					let items = e.detail
					vm.latitude = items.latitude
					vm.longitude = items.longitude
					var demo = new QQMapWX({
						key: '腾讯位置服务key腾讯位置服务官网获取'
					})
					demo.reverseGeocoder({
						location: `${vm.latitude},${vm.longitude}`,
						success: function(res) {
							console.log(res)
							let item = res.result
							vm.cardData.title = item.formatted_addresses.rough
							vm.cardData.category = ""
							vm.cardData.address = item.address
							vm.cardData.latitude = item.location.lat
							vm.cardData.longitude = item.location.lng
							vm.toPs.latitude = item.location.lat;
							vm.toPs.longitude = item.location.lng;
							vm.latitude = item.location.lat
							vm.longitude = item.location.lng
							vm.isShow = true
							let mks = []
							mks.push({
								title: vm.cardData.title,
								id: 1,
								latitude: item.location.lat,
								longitude: item.location.lng,
								callout: {
									content: vm.cardData.title,
									borderColor: 'blue'
								},
								label: {
									content: vm.cardData.title
								}, //地图显示内容
								iconPath: "/static/logos.png"
							})
							vm.markers = mks
						}
					})
				}

				console.log(e)
			},
			search() {
				let vm = this;
				vm.isShow = false;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				demo.getSuggestion({
					keyword:vm.keys,
					region:vm.city,
					success: res=>{
						console.log(res)
						vm.tables = res.data
						vm.isList = true;
					}
				})
			},
			dosome(e) {
				let vm = this;
				vm.polyline = []
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
					from: vm.fromP,
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
			
			getMyLocation() {
				let vm = this;
				uni.getLocation({
					type: 'gcj02',
					success: res => {
						vm.latitude = res.latitude
						vm.longitude = res.longitude
						vm.fromP.latitude = res.latitude
						vm.fromP.longitude = res.longitude
						var demo = new QQMapWX({
							key: '腾讯位置服务key腾讯位置服务官网获取'
						})
						demo.reverseGeocoder({
							location: `${vm.latitude},${vm.longitude}`,
							success: function(ress) {
								console.log(ress)
								let item = ress.result
								let mks = []
								vm.city = item.address_component.district
							}
						})
						
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
	
	.dh{
		top:140rpx;
	}

	.dh-img {
		position: fixed;
		right: 10%;
		bottom: 10%;
		z-index: 50;
		width: 100rpx;
		height: 100rpx;
		animation: gradientBGs 5s ease infinite;
	}

	@keyframes gradientBGs {
		0% {
			height: 100rpx;
			width: 100rpx;
			/* filter: brightness(70%);
			transform: rotateX(0deg) rotateY(0deg); */
			/* transform: rotate(0turn); */
		}


		50% {
			height: 120rpx;
			width: 120rpx;
			/* filter: invert(50%);
			transform: rotateX(0deg) rotateY(180deg); */
			/* transform: rotate(0.5turn); */
		}


		100% {
			height: 100rpx;
			width: 100rpx;
			/* filter: brightness(70%);
			transform: rotateX(0deg) rotateY(0deg); */
			/* transform: rotate(1turn); */
		}
	}
</style>
