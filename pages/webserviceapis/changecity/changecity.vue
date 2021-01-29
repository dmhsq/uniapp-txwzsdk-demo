<template>
	<view>
		<view  class="mian-body" style="z-index: 120;position: fixed;left: 0;top: 0;">
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
		<map id="myMap"   style="width:100%;height:100vh;" :longitude="longitude" :latitude="latitude"
		  scale='18' >
		</map>
	</view>
</template>

<script>
	var QQMapWX = require('../../../common/qqmap-wx-jssdk.js')
	export default {
		data() {
			return {
				longitude:116.400232,
				latitude:39.870091,
				flagsO:0,
				city:"未知"
			}
		},
		onLoad() {
			this.getMyLocation();
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
		methods: {
			getMyLocation() {
				let vm = this;
				uni.getLocation({
					type: 'gcj02',
					success: res => {
						console.log(res)
						vm.latitude = res.latitude
						vm.longitude = res.longitude
						var demo = new QQMapWX({
							key: '腾讯位置服务key腾讯位置服务官网获取'
						})
						demo.reverseGeocoder({
							location: `${vm.latitude},${vm.longitude}`,
							success: function(ress) {
								console.log(ress)
								vm.city = ress.result.address_component.district
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
</style>
