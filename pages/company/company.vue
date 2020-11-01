<template>
	<view>
		<map id="map" :longitude="longitude" :latitude="latitude" :markers="markers" scale="14"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {//114.521978,38.048646
				longitude:'114.521978',
				latitude:'38.048646',
				markers:[]
			}
		},
		onLoad() {
			this.markers=[{
				id:0,
				longitude:this.longitude,
				latitude:this.latitude,
				iconPath:'../../static/img/f8.png',
				callout:{
					content:"美东国际B座",
					bgColor:"#0000AA",
					color:"#FFFFFF",
					padding:5,
					display:"ALWAYS"
				}
			}]
		},
		methods: {
			
		}
	}
</script>

<style scoped lang="less">
#map{
	width: 100vw;
	height: 100vh;
}
</style>
