<template>
	<view>
		<map id="myMap" :markers="markers" @tap="bg" style="width:100%;height:100vh;" :longitude="longitude" :latitude="latitude"
		  scale='20' >
		</map>
		<uni-card v-if="isShow" class="cards" :title="cardData.title" :isFull="true" isShadow='true' note="额外信息" :extra="cardData.category">
			<text class="content-box-text">{{cardData.address}}</text>
			<block slot="footer">
				<view class="footer-box">
					<!-- <view class="" @click.stop="footerClick('喜欢')"><text class="footer-box__item">喜欢</text></view>
					<view class="" @click.stop="footerClick('评论')"><text class="footer-box__item">评论</text></view> -->
					<view><text class="footer-box__item" >去这里</text></view>
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
				markers: [],
				cardData: {
					title: "",
					category: "",
					address: "",
					latitude: 0,
					longitude: 0
				},
				longitude:116.400232,
				latitude:39.870091,
				isShow:false
			}
		},
		methods: {
			bg(e){
				let vm = this;
				if (vm.isShow) {
					vm.isShow = false
				}else{
					var demo = new QQMapWX({
						key: '腾讯位置服务key腾讯位置服务官网获取'
					})
					let items = e.detail
					vm.latitude = items.latitude
					vm.longitude = items.longitude
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
				
			}
			
		}
	}
</script>

<style>
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
