<template>
	<view>
		
		<view class="mx-25 py-3 flex bB-f5" v-for="(item,index) in mainData" :key="index"
		:data-id = "item.id"
		@click="Router.navigateTo({route:{path:'/pages/infoDetail/infoDetail?id='+$event.currentTarget.dataset.id}})">
			<view class="flex-1 pr-3">
				<view class="font-32 avoidOverflow2">{{item.title?item.title:''}}</view>
				<view class="font-24 color9 pt-2">{{item.create_time?item.create_time:''}}</view>
			</view>
			<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="img radius10"></image>
		</view>
		
		
		
		<view style="height: 100rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
				<view>资讯</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/apply/apply'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>申请</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar4.png" mode=""></image>
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
				mainData:[]
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
					menu_id:4
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].create_time = self.mainData[i].create_time.substr(0,10)
						}
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.img{width: 220rpx;height: 150rpx;}
</style>
