<template>
	<view>
		<view class="content">
			<image class="logo" src="../../static/demologo.png"></image>
		</view>
		<uni-list>
			<uni-list-item showArrow clickable title="多人位置实时共享" @click="goDemo(1)"></uni-list-item>
			<uni-list-item showArrow clickable title="移动轨迹" @click="goDemo(2)"></uni-list-item>
			<uni-list-item title="如果反馈多再加案例"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		onLoad() {
			wx.showShareMenu({
			  withShareTicket: true,
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		methods: {
			goDemo(i){
				if(i==1){
					uni.navigateTo({
						url:'../demos/demone/demone'
					})
				}
				if(i==2){
					uni.navigateTo({
						url:'../demos/demotwo/demotwo'
					})
				}
			}
		}
	}
</script>

<style>
	/* .content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color: #C0C0C0;
	} */


	.logo {
		height: 100rpx;
		width: 100rpx;
		margin-top: 100rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 100rpx;
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
			height: 150rpx;
			width: 150rpx;
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

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 400rpx;
		background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
		background-size: 400% 400%;
		animation: gradientBG 5s ease infinite;
	}

	@keyframes gradientBG {
		0% {
			background-position: 0% 50%;
		}

		50% {
			background-position: 100% 50%;
		}

		100% {
			background-position: 0% 50%;
		}
	}
</style>
