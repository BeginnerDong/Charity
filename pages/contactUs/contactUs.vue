<template>
	<view>
		
		<view class="flex px-25 py-4 mb-2 bg-white">
			<image src="../../static/images/contactus-icon1.png" class="icon mr-2"></image>
			<view>联系电话：{{mainData.phone?mainData.phone:''}}</view>
		</view>
		<view class="flex px-25 py-4 mb-2 bg-white">
			<image src="../../static/images/contactus-icon.png" class="icon1 mr-2"></image>
			<view class="flex-1">地址：{{mainData.description?mainData.description:''}}</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id:3
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.icon{width: 26rpx;height: 32rpx;}
.icon1{width: 26rpx;height: 30rpx;}
</style>
