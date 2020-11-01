<template>
	<view class="content">
		  <map id="map"
		       :latitude="latitude"
			   :longitude="longitude"
			   :markers="markers"
			   scale="16"
			   :include-points="includepoints"
			   show-compass="false"
			   show-location="true"
			   ></map>
	</view>
 
</template>

<script>
	// 微信高德地图sdk
	import amap from '../../static/js/amap-wx.js'
	export default {
		data() {
			return {
				mapPlugin:null,
				key:'e604076838202a5f26e7700da68085d1',
				
				latitude:'',
				longitude:'',	
				markers:[],
				includepoints:[]
			}
		},
		onLoad() {
			//实例化高度地图  this.mapPlugin.getRegeo({})用于获取定位位置或者指定位置的详细信息
			this.mapPlugin=new amap.AMapWX({key:this.key});
			let _this=this;
					 
			uni.getLocation({  //获取当前位置
				type:'gcj02',
				success(res) {			 
					_this.latitude=res.latitude;
					_this.longitude=res.longitude;
			 
				}
			});
		 
		  this.mapPlugin.getPoiAround({  //获取当前位置周围餐饮商店POI
			  success:function(e){
				  
				  _this.markers=e.markers;
				  _this.markers.forEach(item=>{
					  item.iconpath='../../static/img/f8.png';
					  item.callout={
						  content:item.name,
						  bgColor:"#6699ff",
						  padding:5,
						  borderRadius:8,
						  display:'ALWAYS' 
					  }
				  }) 
				   
				 // console.log( e);
				  
			  }
		  });
		 


		},
		methods: {

		}
	}
</script>

<style lang="less" scoped>
 #map{
	 width: 100%;
	 height: 800rpx;
 }
</style>
