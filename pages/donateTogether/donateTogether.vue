<template>
	<view>
		
		<view>
			<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="img"></image>
		</view>
		
		<view class="line-h flex4 con">
			<view  class="wh100 radius-5 overflow-h z999">
				<open-data type="userAvatarUrl"></open-data>
			</view>
			<view class="font-w pt-2"><open-data type="userNickName"></open-data></view>
		</view>
		
		<view class="px-25 mt-5">
			<view class="font-36 font-w">捐赠项目</view>
			<view class="py-3 flex">
				<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="img1"></image>
				<view class="flex-1 pl-2 line-h">
					<view class="pb-3">{{mainData.title?mainData.title:''}}</view>
					<view class="font-24 color6 flex-1">{{mainData.execution?mainData.execution:''}}</view>
				</view>
			</view>
		</view>
		
		<view class="px-25 line-h">
			<view class="font-36 mt-5 font-w pb-2">发起者</view>
			<input type="text" v-model="submitData.name" />
			<view class="font-36 mt-5 font-w pb-2">发起说明</view>
			<input type="text" v-model="submitData.explain" value="每个人做一点点,世界就会改变很多" placeholder="" />
			<view class="font-32 mt-5 font-w pb-2">限定每笔捐款金额（元）</view>
			<input type="digit" v-model="submitData.price" placeholder="请填写" />
			<view class="font-32 mt-5 font-w pb-2">设定筹款目标（元）</view>
			<input type="text" v-model="submitData.purpose" placeholder="请填写" />
		</view>
		
		<view class="py-5 mt-5 px-25 font-24 flex">
			<image @click="chooseThis" :src="isChoose?'../../static/images/toapplyfor-icon1.png':'../../static/images/toapplyfor-icon.png'" class="wh30 mr-1"></image>
			
			<view class="font-24 color8">
				你已阅读并同意 <text class="colorM"
				@click="Router.navigateTo({route:{path:'/pages/agreement/agreement?type=1'}})">《用户须知》</text>
			</view>
		</view>
		
		
		<view style="height: 120rpx;"></view>
		<view class="p-2 p-fX bottom-0 shadowT bg-white z10">
			<view class="btn80 Mgb colorf w-100"
			@click="submit">预览</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				submitData:{
					name:'',
					explain:'每个人做一点点,世界就会改变很多',
					price:'',
					purpose:''
				},
				isChoose:false
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			chooseThis(){
				const self = this;
				self.isChoose = !self.isChoose
			},
			
			submit(){
				const self = this;
				if(!self.isChoose){
					self.$Utils.showToast('请阅读并同意用户须知','none')
					uni.setStorageSync('canClick', true);
					return
				};
				var pass = self.$Utils.checkComplete(self.submitData);
				if(pass){
					uni.setStorageSync('togetherSubmit',self.submitData)
					self.Router.navigateTo({route:{path:'/pages/together/together?id='+self.id}})
				}else{
					self.$Utils.showToast('请补充信息','none')
				}
			},
			
			getMainData(isNew) {
				var self = this;
				var postData = {};
				postData.searchItem = {
					id:self.id
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};	
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.projectGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.img{height: 440rpx;}
.con{margin-top: -50rpx;}
.img1{width: 160rpx;height: 120rpx;border-radius: 10rpx;}
.h120{height: 120rpx;}

input{width: 100%;height: 66rpx;font-size: 24rpx;border-radius: 10rpx;border: 1px solid #DDDDDD;padding: 0 20rpx;box-sizing: border-box;}
</style>
