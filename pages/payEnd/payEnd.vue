<template>
	<view>
		
		<view class="mt-5 mx-25 p-r">
			<image src="../../static/images/compaete-img.png" mode="widthFix"></image>
			<view class="p-aXY d-flex flex-column a-center pt-5">
				<image :src="mainData.user&&mainData.user[0]?mainData.user[0].headImgUrl:''" class="wh110 overflow-h" style="border-radius: 50%;"></image>
				<view class="font-44 font-w colorR pt-2">{{mainData.price?mainData.price:''}}元</view>
				<view class="pt-5 mt-3">感谢您此次募捐</view>
			</view>
		</view>
		
		<view class="btn80 Mgb colorf"
		@click="Router.navigateTo({route:{path:'/pages/certificate/certificate?id='+mainData.id}})">查看捐款证书</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			if(options.id){
				self.id = options.id;
			};
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				postData.getAfter = {
					user: {
						tableName: 'User',
						searchItem: {
							status:1,
							user_type:0
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.btn80{width: 500rpx;margin-top: 60rpx;}
</style>
