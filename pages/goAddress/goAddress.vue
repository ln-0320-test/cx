<template>
	<view>
		<map id="map" :longitude="longitude" :latitude="latitude"     
					  scale="14"
		              :markers="markers" 
					  :polyline="polyline">
					  </map>
					  <view>
						  <label>距离:{{distance}}米</label>
						   <label>打车约:{{taxi_cost}}元</label>
						    <label>速度:{{strategy}}</label>
					  </view>
	</view>
</template>

<script>
	import Amap from '../../static/js/amap-wx.js'
	export default {
		data() {
			return {
				longitude:'',
				latitude:'',
				polyline:[],
				key:'e604076838202a5f26e7700da68085d1',
				markers:[],
				amap:null,
				distance:'',
				taxi_cost:'',
				strategy:''
			}
		},
		onLoad(e) {
			let _this=this;
			console.log(e);
			//当前位置
			uni.getLocation({
				success(res) {
					_this.latitude=res.latitude;
					_this.longitude=res.longitude;
					
					let obj=e.location.split(',');
					_this.markers=[
						{
							id:0,
							longitude:res.longitude,
							latitude:res.latitude,
							iconPath:'../../static/img/d3.png',
							width:30,
							height:40
						},
						{
							id:parseInt(e.id),
							longitude:obj[0],
							latitude:obj[1],
							 
							iconPath:'../../static/img/f8.png',
							width:30,
							height:40
						}
					];
				//驾车路线
				_this.amap=new Amap.AMapWX({key:_this.key});
				_this.amap.getDrivingRoute({
					   origin: _this.longitude+","+_this.latitude,
					   destination:e.location,
					   success(data){
						   console.log(data);
						   var points=[];
						   _this.distance=data.paths[0].distance;
						   _this.taxi_cost=data.taxi_cost;
				           _this.strategy=data.paths[0].strategy;
						   let steps=data.paths[0].steps;
						   if(data.paths && data.paths[0] && steps){
							 
							 // for(let i=0;i<steps.length;i++){
								//  let po=steps[i].polyline.split(";");
								//  for(let j=0;j<po.length;j++){
								// 	 points.push({
								// 		 latitude:parseFloat(po[j].split(",")[1]),
								// 		 longitude:parseFloat(po[j].split(",")[0])
										 
								// 	 });
								//  }
							 // }  
							 steps.forEach(item=>{
								 let po=item.polyline.split(";");
								 po.forEach(el=>{
									 points.push({
										 longitude:el.split(",")[0],
										 latitude:el.split(",")[1]
									 })
								 })
								 
							 })
							   
						   }
						  console.log(points);
						  
						  //设置polyline
						 
						  _this.polyline=[
						  				 {
						  					points:points,
						  					color: "#0000AA",
						  					width: 6,	
						  				 }
						                ];
						   
					   }
				});				
								
				}
			});
		
			
		},
		methods: {
			
		}
	}
</script>

<style scoped lang="less">
#map{
	width: 100%;
	height: 800rpx;
}
</style>
