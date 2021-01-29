<template>
	<view>
		<view class="content">
			<image class="logo" src="../../../static/runslogo.png"></image>
		</view>
		<uni-list>
			<uni-list-item showArrow clickable thumb="../../static/jing.png" title="静态" @click="goDemo(1)"></uni-list-item>
			<uni-list-item showArrow clickable thumb="../../static/dong.png" title="实时" @click="goDemo(2)"></uni-list-item>
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

		},
		methods: {
			goDemo(i) {
				if (i == 1) {
					uni.navigateTo({
						url: '../../demotwos/static/static'
					})
				}
				if (i == 2) {
					uni.navigateTo({
						url: '../../demotwos/dynamic/dynamic'
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
		background-color: #0055ff;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 100rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 80rpx;
	}
</style>
