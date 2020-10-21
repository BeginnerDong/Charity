<template>
	<view class="px-25 bg-white">
		
		<view class="py-3 bB-f5" v-for="(item,index) in mainData" :key="index">
			<view class="flex">
				<image :src="item.project&&item.project[0]&&item.project[0].mainImg&&item.project[0].mainImg[0]?item.project[0].mainImg[0].url:''" class="img radius10"></image>
				<view class="h160 flex5 flex-1 pl-2">
					<view class="font-32">{{item.project&&item.project[0]?item.project[0].title:''}}</view>
					<view class="font-26 color9">已捐金额(元)： <text class="colorR">{{item.order?Utils.fmoney(item.order.allPrice,2):0}}</text></view>
				</view>
			</view>
			<view class="font-24 color8 bg-f5 p-2 radius10 flex mt-2"
			:data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/certificate/certificate?id='+$event.currentTarget.dataset.id}})">
				<view class="flex-1 line-h-lg">
					<view>捐款次数：1次</view>
					<view>捐款时间：{{item.create_time}}</view>
					<view>证书编号：{{item.passage1}}</view>
				</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
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
					pay_status:1,
					type:1
				};
				postData.getAfter = {
					project: {
						tableName: 'Project',
						searchItem: {
							status:1,
						},
						middleKey: 'project_id',
						key: 'id',
						condition: 'in',
					},
					order:{
						tableName: 'Order',
						searchItem: {
							status:1,
							pay_status:1,
							user_type:0,
							type:1
						},
						middleKey: 'project_id',
						key: 'project_id',
						condition: 'in',
						compute:{
						  allPrice:[
						    'sum',
						    'price',
						    {
						     status:1,
						     pay_status:1,
						     user_type:0,
							 type:1
						    }
						  ]
						},
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.img{width: 220rpx;height: 160rpx;}
.h160{height: 160rpx;}
</style>
