<template>
	<view>
		<map id="myMap" :markers="markers" style="width:100%;height:90vh;" :longitude="longitude" :latitude="latitude" scale='16'>
		</map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				markers: [{
					title: "主要",
					id: 11,

					latitude: 33.404659,
					longitude: 115.089099,
					label:{content:"主要",color:"#0000ff"},
					callout: {
						content: "主要",
						borderColor: 'blue'
					},
					iconPath: "../../../static/mainPeo.png"
				},{
					title: "普通1",
					id: 112,

					latitude: 33.403977,
					longitude: 115.088245,
					label:{content:"普通1",color:"#000000"},
					callout: {
						content: "普通1",
						borderColor: 'blue'
					},
					iconPath: "../../../static/somePeo.png"
				},{
					title: "普通2",
					id: 113,
					label:{content:"普通2",color:"#000000"},
					latitude: 33.404098,
					longitude: 115.087183,
					callout: {
						content: "普通2",
						borderColor: 'blue'
					},
					iconPath: "../../../static/somePeo.png"
				}],
				latitude: 33.404659,
				longitude: 115.089099
			}
		},
		methods: {

		}
	}
</script>

<style>

</style>
