<template>
	<view>
		
		<view class="Mgb py-4 colorf line-h flex">
			<view class="flex4 w-33 rb">
				<view class="font-36 font-w pb-2">{{info.total_num}}</view>
				<view class="font-22">发起一起捐/次</view>
			</view>
			<view class="flex4 w-33 rb">
				<view class="font-36 font-w pb-2">{{info.order_num}}</view>
				<view class="font-22">参与伙伴/位</view>
			</view>
			<view class="flex4 w-33">
				<view class="font-36 font-w pb-2">{{info.total_price}}</view>
				<view class="font-22">共筹善款/元</view>
			</view>
		</view>
		
		<view class="py-3 px-25 bB-f5" v-for="(item,index) in mainData" :key="index">
			<view class="flex">
				<image :src="item.project&&item.project[0]&&item.project[0].mainImg&&item.project[0].mainImg[0]?item.project[0].mainImg[0].url:''" class="img radius10"></image>
				<view class="h160 flex5 flex-1 pl-2">
					<view class="font-32">{{item.project&&item.project[0]?item.project[0].title:''}}</view>
					<view class="font-26 color9">已捐金额(元)： <text class="colorR">{{item.order?Utils.fmoney(item.order.allPrice,2):0}}</text></view>
				</view>
			</view>

			<view class="font-24 color8 bg-f5 p-2 radius10 flex mt-2">
				<view class="flex-1 line-h-lg">
					<view>捐款人数：{{item.order?item.order.count:0}}</view>
					<view>发起时间：{{item.create_time}}</view>
				</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				info:{
					order_num:0,
					total_num:0,
					total_price:0
				},
				mainData:[],
				Utils:this.$Utils
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getTeamNum','getMainData'], self);
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
			
			getTeamNum() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res) {
					if (res.solely_code == 100000) {
						self.info  = res.info
					};
					self.$Utils.finishFunc('getTeamNum');
				};
				self.$apis.getTeamNum(postData, callback);
			},
			
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
					order: {
						tableName: 'Order',
						searchItem: {
							status:1,
							pay_status:1,
							user_type:0,
							type:1
						},
						middleKey: 'id',
						key: 'team_id',
						condition: 'in',
						compute:{
						  allPrice:[
						    'sum',
						    'price',
						    {
						      status:1,pay_status:1,user_type:0,type:1
						    }
						  ],
						  count:[
						    'count',
						    'count',
						    {
						      status:1,pay_status:1,user_type:0,type:1
						    }
						  ]
						},
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.teamGet(postData, callback);
			},
			
			
		}
	}
</script>

<style scoped>
.img{width: 220rpx;height: 160rpx;}
.h160{height: 160rpx;}
</style>
