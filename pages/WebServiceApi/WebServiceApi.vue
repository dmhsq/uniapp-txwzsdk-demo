<template>
	<view>
		<view class="content">
			<!-- https://mapapi.qq.com/web/lbs/video/wemap/api_banner.mp4 -->
			<view class="img-box">
				<image class="img-webapi" src="../../static/webservicelogo.png"></image>
			</view>
			<video class="videoW" src="https://mapapi.qq.com/web/lbs/video/wemap/api_banner.mp4" loop="true" :controls="false"
			 autoplay="true" show-play-btn="false" show-center-play-btn="false" show-progress="false" show-fullscreen-btn="false"
			 object-fit="cover"></video>
			<!-- :style="{backgroundColor:bgColor}" -->

		</view>
		<uni-list>
			<uni-list-item showArrow clickable title="综合DEMO" @click="goUse(8)"></uni-list-item>
			<uni-list-item showArrow clickable title="城市选择" @click="goSelCity()"></uni-list-item>
			<uni-list-item showArrow clickable title="搜索地点" @click="goUse(1)"></uni-list-item>
			<uni-list-item showArrow clickable title="搜索提醒" @click="goUse(2)"></uni-list-item>
			<uni-list-item showArrow clickable title="地图标记" @click="goUse(9)"></uni-list-item>
			<uni-list-item showArrow clickable title="地图放缩" @click="goUse(11)"></uni-list-item>
			<uni-list-item showArrow clickable title="改变城市" @click="goUse(10)"></uni-list-item>
			<uni-list-item showArrow clickable title="逆地址解析" @click="goUse(3)"></uni-list-item>
			<uni-list-item showArrow clickable title="地址解析" @click="goUse(5)"></uni-list-item>
			<uni-list-item showArrow clickable title="路线规划" @click="goUse(4)"></uni-list-item>
			<uni-list-item showArrow clickable title="距离计算" @click="goUse(6)"></uni-list-item>
			<uni-list-item showArrow clickable title="城市信息" @click="goUse(7)"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bgColor: "#636363"
			}
		},
		onLoad() {
			wx.showShareMenu({
			  withShareTicket: true,
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		methods: {
			goSelCity() {
				uni.navigateTo({
					url: '../webserviceapis/choices/choices'
				})
			},
			goUse(i) {
				if (i == 1) {
					uni.navigateTo({
						url: '../webserviceapis/search/search'
					})
				}
				if (i == 2) {
					uni.navigateTo({
						url: '../webserviceapis/keys/keys'
					})
				}
				if (i == 3) {
					uni.navigateTo({
						url: '../webserviceapis/reverse/reverse'
					})
				}
				if (i == 4) {
					uni.navigateTo({
						url: '../webserviceapis/plan/plan'
					})
				}
				if (i == 5) {
					uni.navigateTo({
						url: '../webserviceapis/analysis/analysis'
					})
				}
				if (i == 6) {
					uni.navigateTo({
						url:'../webserviceapis/distance/distance'
					})
				}
				if (i == 7) {
					uni.navigateTo({
						url:'../webserviceapis/citys/citys'
					})
				}
				if (i == 8) {
					uni.navigateTo({
						url:'../webserviceapis/demoall/demoall'
					})
				}
				if (i == 9) {
					uni.navigateTo({
						url:'../webserviceapis/point/point'
					})
				}
				if (i == 10) {
					uni.navigateTo({
						url:'../webserviceapis/changecity/changecity'
					})
				}
				if (i == 11) {
					uni.navigateTo({
						url:'../webserviceapis/changesize/changesize'
					})
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

	.img-webapi {
		width: 200rpx;
		height: 200rpx;
		margin-top: 20rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.img-box {

		z-index: 99;
		position: absolute;
	}

	.videoW {
		width: 100%;
	}
</style>
