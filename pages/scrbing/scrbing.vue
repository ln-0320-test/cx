<template>
	<view>
		<view class="search">
			<input type="text" id="inputTxt" v-model="inputTxt" @input="getKey" />
			<button type="primary" @click="goThere">去这里</button>
		</view>
		<view class="keyList" v-if="tips.length">
			<label v-for="(item,index) in tips" :key='index' @click="getInput(index)">
			<text>{{item.name}}</text>
			<text>{{item.district}}-{{item.address}}</text>
			</label>
		</view> 
        <!-- 地图 -->
		<map id="map" :longitude="longitude" :latitude="latitude" 
		              :markers="markers" @markertap="delMark" 
					  scale="14"
					  show-location="true">
			
		</map>
		
	</view>
</template>

<script>
	import Amap from '../../static/js/amap-wx.js'
	export default {
		data() {
			return {
				longitude:'',
				latitude:'',
				inputTxt:'',
				amap:null,
				key:'e604076838202a5f26e7700da68085d1',
				tips:[],
				point:[],
				markers:[],
				keyObj:{}
				 
			}
		},
		onLoad() {
			let _this=this;
			uni.getLocation({  //获取当前位置
				success(res){
					_this.latitude=res.latitude;
					_this.longitude=res.longitude;
					_this.point.push(res.longitude+','+res.latitude);  //起点
					console.log(_this.point);
					_this.markers.push({
						id:0,
						longitude:res.longitude,
						latitude:res.latitude,
						iconPath:'../../static/img/d3.png',
						width:30,
						height:40 
					});	
				}
			});
		 
			
		},
		methods: {
			getKey(){
				console.log(this.inputTxt);
				let _this=this;
				this.amap=new Amap.AMapWX({key:this.key});
				this.amap.getInputtips({
					keywords:_this.inputTxt,
					location:'',
					success(res){
						//console.log(res.tips);
						_this.tips=res.tips;	 	
					}
				});
			},
			getInput(e){
				//console.log(e);
				this.keyId=parseInt(e);
				this.inputTxt=this.tips[e].name;
				this.keyObj=this.tips[e]; 
				this.tips=[];	
			} ,
			goThere(){		 
				console.log(this.keyObj);  
				// let keywords="id="+this.keyObj.id+"&location="+this.keyObj.location+"&name="+this.keyObj.name+
				//               "&district="+this.keyObj.district+"&address="+this.keyObj.address;
			    let keywords=`id=${this.keyObj.id}&location=${this.keyObj.location}`;
				uni.navigateTo({
					url:'../goAddress/goAddress?'+keywords  
				})
			}
		}
	}
</script>

<style lang="less" scoped>
.search{
	width: 92%;
	padding: 0 4%;
	height: 60rpx;
	display: flex;
	justify-content: space-around;
	margin:20rpx 0;
	align-items: center;
	& input{
		border: 1px solid #555555;
		width: 80%;
	}
	& button{
		width: 20%;
		font-size: 26rpx;
	}
}
.keyList{
	width: 92%;
	margin: 0 4%;
	background-color: #FFFFFF;
	position:absolute;
	top:80rpx;
	z-index: 999;
    border: 1px solid;
	& label{
		width: 100%;
		height: 100rpx;
		display: flex;
		flex-direction: column;
		border-bottom: 1px solid #2C405A;
	}
	& label text:first-child{
		width: 100%;
		height: 60rpx;
	}
	& label text:last-child{
		width: 100%;
		height: 40rpx;
		color: #808080;
	}
}
#map{
	width: 100%;
	height: 800rpx;
}
</style>
