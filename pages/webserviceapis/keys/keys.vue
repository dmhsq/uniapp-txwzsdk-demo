<template>
	<view>
		<view class="mian-body">
			<uni-nav-bar :fixed="false" color="#333333" background-color="#FFFFFF" right-icon="search">
				<block slot="left"  @click="onClickLeft()">
					<view class="city" >
						<view><text class="uni-nav-bar-text">{{ city }}</text></view>
						<uni-icons type="arrowdown" color="#333333" size="22" style="padding-top: 12rpx;" />
					</view>
				</block>
				<view class="input-view">
					<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
					<input confirm-type="search" class="nav-bar-input" type="text" placeholder="输入搜索关键词" v-model="keys" @input="search" @confirm="search">
				</view>
			</uni-nav-bar>
		</view>
		<uni-list>
			<uni-list-item v-for="item in tables" :title="item.title" :note="item.address" showArrow clickable="" :rightText="item.category" @click="goMap(item)"/>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				city: "沈丘",
				keys: "",
				tables: []
				
			}
		},
		methods: {
			goMap(item){
				let items = JSON.stringify(item)
				uni.navigateTo({
					url:`../../webviewty/webviewty?item=${items}`
				})
			},
			search(){
				let vm = this;
				var demo = new QQMapWX({
					key: '腾讯位置服务key腾讯位置服务官网获取'
				})
				demo.getSuggestion({
					keyword:vm.keys,
					region:vm.city,
					success: res=>{
						console.log(res)
						vm.tables = res.data
					}
				})
			},
			onClickLeft() {
				uni.navigateTo({
					url:'../../selectwz/selectwz'
				})
			},
			showCity() {
				uni.navigateTo({
					url:'../../selectwz/selectwz'
				})
			},
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
</style>
