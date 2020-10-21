<template>
	<view>
		
		<view class="Mgb py-4 colorf line-h flex">
			<view class="flex4 w-33 rb">
				<view class="font-36 font-w pb-2">{{projectData.team?projectData.team.count:0}}</view>
				<view class="font-22">发起一起捐/次</view>
			</view>
			<view class="flex4 w-33 rb">
				<view class="font-36 font-w pb-2">{{projectData.order?projectData.order.count:0}}</view>
				<view class="font-22">参与伙伴/次</view>
			</view>
			<view class="flex4 w-33">
				<view class="font-36 font-w pb-2">{{projectData.order?projectData.order.allPrice:0}}</view>
				<view class="font-22">共筹善款/元</view>
			</view>
		</view>
		
		<view class="bg-white px-25">
			
			<view class="d-flex a-start mx-25 py-4 bB-f5" v-for="(item,index) in mainData" :key="index">
				<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" class="wh100 radius-5"></image>
				<view class="pt-1 pl-2 line-h d-flex j-sb a-start flex-1">
					<view>
						<view class="font-30 pb-2">{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
						<view class="font-26 pb-3">和{{item.order?item.order.count:0}}位小伙伴筹得{{item.order?item.order.allPrice:0}}元</view>
						<view class="font-22 color9">{{item.explain?item.explain:''}}</view>
					</view>
					<view class="font-22 color9">{{Utils.formatMsgTime(item.create_time)}}</view>
				</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				projectData:{},
				mainData:[],
				Utils:this.$Utils
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getProjectData'], self);
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
					project_id:self.id,
					user_type:0
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					order: {
						tableName: 'Order',
						searchItem: {
							status:1,
							pay_status:1,
							user_type:0
						},
						middleKey: 'id',
						key: 'team_id',
						condition: 'in',
						compute:{
						  allPrice:[
						    'sum',
						    'price',
						    {
						      status:1,
						      pay_status:1,
						      user_type:0
						    }
						  ],
						  count:[
						    'count',
						    'count',
						    {
						      status:1,
						      pay_status:1,
						      user_type:0
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
			
			getProjectData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					order: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Order',
						searchItem: {
							status:1,
							pay_status:1,
							user_type:0,
							team_id:['>',0]
						},
						middleKey: 'id',
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
								team_id:['>',0]
						    }
						  ],
						  count:[
						    'count',
						    'count',
						    {
						      status:1,
						      pay_status:1,
						      user_type:0,
							  team_id:['>',0]
						    }
						  ]
						},
					},
					team: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Team',
						searchItem: {
							thirdapp_id:2
						},
						middleKey: 'id',
						key: 'project_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      thirdapp_id:2
						    }
						  ]
						},
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.projectData = res.info.data[0]
					};
					self.$Utils.finishFunc('getProjectData');
				};
				self.$apis.projectGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
