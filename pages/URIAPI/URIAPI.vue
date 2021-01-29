<template>
	<view>
		<view class="content">
			<!-- :style="{backgroundColor:bgColor}" -->
			<view class="test-data">
				<view>腾讯位置服务使用次数实时新增:</view>
				<view class="text-area">
					<text class="title">{{data}}</text>
				</view>
			</view>
			
			<image class="img-uriapi" src="https://lbs.qq.com/img/bg_data@2x.36840526.jpg"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data:0,
				timer:"",
				timers:""
			}
		},
		mounted() {
			wx.showShareMenu({
			  withShareTicket: true,
			  menus: ['shareAppMessage', 'shareTimeline']
			})
			this.getData();
			let vm = this;
			this.timer = setInterval(function(){
				clearInterval(vm.timers)
				vm.getData()
			},10000)
			
		},
		beforeDestroy() {
			clearInterval(this.timer)
			// clearInterval(this.timers)
		},
		methods: {
			getData(){
				let vm = this;
				uni.request({
					url:'https://xingyun.map.qq.com/api/getStatics.php?type=0&source=all&interface=all&count=10&delay=60',
					success:function(res){
						let datas = res.data;
						let sumTime = 0;
						datas.forEach((item,index)=>{
							sumTime+=parseInt(item.requests)
						})
						let nes = parseInt(sumTime/100);
						vm.timers = setInterval(function(){
							vm.data+=nes
						},100)
					}
				})
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

	.img-uriapi {
		width: 100%;
		margin-top: 20rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}
	.text-area {
		display: flex;
		justify-content: center;
		padding-top: 40rpx;
		color: white;
		padding-bottom: 40rpx;
	}
	.title {
		font-size: 60rpx;
		color: #0055ff;
	}
	.test-data{
		position: fixed;
		z-index: 99;
		color: white;
	}
</style>
