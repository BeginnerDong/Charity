<template>
	<view>
		
		<view class="flex px-25 py-4">
			<view  class="wh110 radius-5 overflow-h" @click="addScore">
				<open-data type="userAvatarUrl"></open-data>
			</view>
			
			<!-- <image src="../../static/images/my-img.png" class="wh110 radius-5"></image> -->
			<view class="font-32 font-w pl-2"><open-data type="userNickName"></open-data></view>
		</view>
		<view class="flex colorf line-h Mgb radius10 mx-25 shadowM py-5 p-r overflow-h mb-3 top">
			<image src="../../static/images/my-icon.png" class="icon p-a"></image>
			<view class="w-50 flex4 p-r">
				<view class="font-44 font-w pb-4">{{allMoney}}</view>
				<view class="font-24">捐款金额/元</view>
			</view>
			<view class="w-50 flex4 p-r">
				<view class="font-44 font-w pb-4">{{num}}</view>
				<view class="font-24">捐款次数</view>
			</view>
		</view>
		
		<view class="font-30 line-h mx-25 flex"
		@click="Router.navigateTo({route:{path:'/pages/user-program/user-program'}})">
			<image src="../../static/images/my-icon5.png" class="wh36 mr-3"></image>
			<view class="flex1 flex-1 bB-f5 py-4">
				<view>捐助项目</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
		</view>
		<view class="font-30 line-h mx-25 flex"
		@click="Router.navigateTo({route:{path:'/pages/togetherOrder/togetherOrder'}})">
			<image src="../../static/images/my-icon4.png" class="wh36 mr-3"></image>
			<view class="flex1 flex-1 bB-f5 py-4">
				<view>发起的一起捐</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
		</view>
		<view class="font-30 line-h mx-25 flex"
		@click="Router.navigateTo({route:{path:'/pages/mouth/mouth'}})">
			<image src="../../static/images/my-icon2.png" class="wh36 mr-3"></image>
			<view class="flex1 flex-1 bB-f5 py-4">
				<view>月捐管理</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
		</view>
		<view class="font-30 line-h mx-25 flex"
		@click="Router.navigateTo({route:{path:'/pages/applyList/applyList'}})">
			<image src="../../static/images/my-icon3.png" class="wh36 mr-3"></image>
			<view class="flex1 flex-1 bB-f5 py-4">
				<view>申请列表</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
		</view>
		<view class="font-30 line-h mx-25 flex"
		@click="Router.navigateTo({route:{path:'/pages/contactUs/contactUs'}})">
			<image src="../../static/images/my-icon1.png" class="wh36 mr-3"></image>
			<view class="flex1 flex-1 bB-f5 py-4">
				<view>联系我们</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
		</view>
		
		
		
		
		<view style="height: 140rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/information/information'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>资讯</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/apply/apply'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>申请</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar4-a.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				allMoney:0,
				num:0
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getOrderData'], self);
		},
		methods: {
			
			addScore() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					count:10000,
					thirdapp_id:2,
					type:3,
					trade_info:'系统增送',
					account:1
				};
				var callback = function(res) {
					
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			getOrderData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no,
					pay_status:1,
					type:1
				};
				postData.compute = {
					allPrice: [
						'sum',
						'price',
						{user_no:uni.getStorageSync('user_info').user_no,pay_status:1,type:1}
					],
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.allMoney =self.$Utils.fmoney(res.info.compute.allPrice,2);
					};
					self.num  = res.info.total;
					self.$Utils.finishFunc('getOrderData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.top{box-shadow: 0 4rpx 20rpx 0 rgba(214,99,19,0.5);}
.icon{width: 178rpx;height: 200rpx;right: -20rpx;top: -30rpx;}
</style>
