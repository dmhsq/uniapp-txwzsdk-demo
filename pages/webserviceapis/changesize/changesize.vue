<template>
	<view>
		<map id="myMap" @regionchange="test"  style="width:100%;height:100vh;" :longitude="longitude" :latitude="latitude"
		 :scale='scale'>
		</map>
		<view style="position: fixed;top: 5%;right: 15%;width: 300rpx;height: 50rpx;">
			<image @click="changSize('down')" src="../../../static/jianh.png" style="width: 30rpx;height: 30rpx;float: left;position: relative;top: 20rpx;"></image>
			<slider style="width: 200rpx;padding-left: 15rpx;" :value="scale" @change="sliderChange" @changing="sliderChange" min="3" max="20" activeColor="#006d00" backgroundColor="#000000"
			 block-color="#8A6DE9" block-size="15" />
			<image @click="changSize('up')" src="../../../static/jiah.png" style="width: 30rpx;height: 30rpx;position: absolute;right: 0;top: 20rpx;"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				latitude: 39.870091,
				longitude: 116.400232,
				scale: 18
			}
		},
		methods: {
			showSize(){
				uni.showToast({
					title:'缩放等级'+this.scale,
					icon:'none',
					duration:1500
				})
			},
			test(e){
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
			}
		}
	}
</script>

<style>

</style>
