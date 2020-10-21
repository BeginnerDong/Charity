<template>
	<view>
		
		<view class="mx-25 mb-3 line-h p-r" v-for="(item,index) in mainData" :key="index"
		:data-id = "item.id"
		@click="Router.navigateTo({route:{path:'/pages/applyProgram/applyProgram?id='+$event.currentTarget.dataset.id}})">
			<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="img radius10"></image>
			<view class="mx-3 bg-white radius10 shadow p-3 p-r txt">
				<view class="font-32 font-w avoidOverflow2">{{item.title}}</view>
				<!-- <view class="font-22 pt-4">已筹款金额(元)： <text class="colorR font-30 font-w">0</text></view> -->
			</view>
			<view class="o5 font-24 colorf p-a left-0 top-0 sign">{{item.type==1?'个人':'项目'}}·{{item.check_status==0?'审核中':(item.check_status==1?'审核通过':'审核拒绝')}}</view>
			<!-- <view class="o5 font-24 colorf p-a left-0 top-0 sign">项目·进行中</view> -->
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				Utils:this.$Utils
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData(isNew) {
				var self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.applyGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.img{width: 100%;height: 320rpx;}
.txt{margin-top: -78rpx;}
.sign{border-radius: 10rpx 0 10rpx 0;line-height: 46rpx;display: inline-block;padding: 0 10rpx;}
</style>
