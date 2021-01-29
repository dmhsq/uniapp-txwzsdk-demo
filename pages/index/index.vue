<template>
	<view class="content">
		<!-- https://mapapi.qq.com/web/lbs/video/wemap/WeMap01.mp4 -->
		<video class="videoC" src="https://mapapi.qq.com/web/lbs/video/wemap/WeMap01.mp4" loop="true" :controls="false" 
		 autoplay="true" show-play-btn="false" show-center-play-btn="false" show-progress="false" show-fullscreen-btn="false"
		 object-fit="cover">
		</video>
		<!-- <image src="../../static/wz.gif" class="gifs"></image> -->
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view class="text-area">
			<text class="title-sub"> Tencent location service is very easy to use </text>
		</view>
		<view class="text-area-t">
			<text class="title-sub-t" @click="goApi()">CSDN: 代码哈士奇</text>
		</view>
		<view class="text-areas">
			<text class="title">视频来源为腾讯位置服务官网</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '腾讯位置服务非常好用',
				isO: false
			}
		},
		onLoad() {
			uni.showShareMenu({
				withShareTicket:true
			})
			wx.showShareMenu({
			  withShareTicket: true,
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		methods: {
			goApi() {
				var pages = getCurrentPages();
				console.log(pages[0].is)
				// uni.navigateTo({
				// 	url:'../webviewty/webviewty'
				// })
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

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
		padding-bottom: 40rpx;
	}

	.text-areas {
		padding-top: 30rpx;
		display: flex;
		justify-content: center;
		padding-bottom: 40rpx;
	}

	.text-area-t {
		padding-top: 50rpx;
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}

	.title-sub {
		font-size: 30rpx;
		color: #000000;
	}

	.title-sub-t {
		font-size: 42rpx;
		color: #000000;
	}

	.videoC {
		width: 400rpx;
		height: 400rpx;
		border-radius: 300rpx;
		margin-top: 100rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 40rpx;
	}

	.gifs {
		width: 292rpx;
		height: 40rpx;
		padding-bottom: 30rpx;
	}
</style>
