<template>
	<view>
		<view class="content">
			<image class="logo" src="../../../static/guidlogo.png"></image>
		</view>
		<uni-list>
			<uni-list-item showArrow clickable thumb="../../static/mainlogo.png" title="主要人物" @click="goDemo(1)"></uni-list-item>
			<uni-list-item showArrow clickable thumb="../../static/somelogo.png" title="次要人物"@click="goDemo(2)"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			goDemo(i){
				if(i==1){
					uni.navigateTo({
						url:'../../demones/guide/guide'
					})
				}
				if(i==2){
					uni.navigateTo({
						url:'../../demones/tourist/tourist'
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
		background-color: #C0C0C0;
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
