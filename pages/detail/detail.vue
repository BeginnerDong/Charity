<template>
	<view>
		
		<view class="px-2 py-3">
			<view class="font-44 pb-5 font-w mb-1">{{mainData.title?mainData.title:''}}</view>
			<view class="flex line-h">
				<view class="flex4 w-50 rb">
					<view class="font-50 colorR font-w">{{mainData.order?Utils.fmoney(mainData.order.allPrice,2):0}}</view>
					<view class="font-22 pt-2">已经筹到(元)</view>
				</view>
				<view class="flex4 w-50">
					<view class="font-50 colorR font-w">{{mainData.order?mainData.order.count:0}}</view>
					<view class="font-22 pt-2">捐款次数</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-3 p-r"  >
			<view class="font-34 line-h pl-2 title">求助人故事</view>
			<view class="font-30 py-2" :style="{height:showHeight+'px'}" style="overflow: hidden;">
				<view  id="content" class="content">
					<view class="ql-editor" style="padding:0;" v-html="mainData.content">
					</view>
				</view>
			</view>
			
			
			<view class="py-2 bg-white p-aX bottom-0" :class="open?'':'zk'" v-show="contentHeight>200">
				<view class="btn50 bg-f5 flex0" @click="openShow()">
					<image src="../../static/images/icon.png" class="wh20 mr-1" v-if="open"></image>
					<image src="../../static/images/icon1.png" class="wh20 mr-1" v-else></image>
					<view class="font-26">{{open?'收起':'展开全文'}}</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-25 py-4">
			<view class="font-34 line-h pl-2 title">进展反馈</view>
			<view class="mt-4 pl-2 p-r bL" v-if="index==0" v-for="(item,index) in mainData.process" :key="index">
				<image src="../../static/images/details-icon.png" class="wh16 p-a"></image>
				<view class="font-26 color6">{{item.create_time.substr(0,10)}}</view>
				<view>
					<view class="content ql-editor" style="padding:0;" v-html="item.content">
					</view>
				</view>
				<view class="flex flex-wrap">
					<image v-for="(c_item,c_index) in item.mainImg" :key="c_index" :src="c_item.url" class="img"></image>
				</view>
			</view>
			<view class="pl-2 py-5 p-r line-h bL flex1"
			@click="Router.navigateTo({route:{path:'/pages/progress/progress'}})">
				<image src="../../static/images/details-icon.png" class="wh16 p-a ee"></image>
				<view>查看近期反馈</view>
				<image src="../../static/images/my-icon6.png" class="R-icon"></image>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		
		<view class="px-25 py-4">
			<view class="font-34 line-h pl-2 title">筹款记录</view>
			<view class="py-4 p-r line-h font-26 flex1" v-if="teamData.length>0"
			@click="Router.navigateTo({route:{path:'/pages/record/record?id='+mainData.id}})"> 
				<view>{{totalTeam}}人发起了一起捐</view>
				<image src="../../static/images/my-icon6.png" class="R-icon"></image>
			</view>
			<view class="flex1" v-if="teamData.length>0">
				<view class="flex4 font-26" v-for="(item,index) in teamData" :key="index"
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/goTogether/goTogether?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" class="wh100 radius-5 mb-1"></image>
					<view>{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
					<view>携手{{item.order?item.order.count:0}}人</view>
				</view>
			</view>
			<view class="font-24 pt-4" v-if="orderData.length>0">
				<view class="flex py-1" v-for="(item,index) in orderData" :key="index">
					<image :src="item.headImgUrl" class="wh40 radius-5"></image>
					<view class="flex flex-1 px-1">
						<text class="name avoidOverflow">{{item.nickname}}</text>捐出
						<text class="colorR">{{item.price}}</text>元
					</view>
					<view class="color9">{{Utils.formatMsgTime(item.create_time)}}</view>
				</view>
			</view>
			<view class="font-24 color pt-2" v-if="totalOrder>10" @click="nextPage">换一批 ></view>
		</view>
		<view class="f5Bj-H20"></view>
		
		
		<view class="px-25 py-4 line-h">
			<view class="font-34 line-h pl-2 pb-1 title">执行机构信息</view>
			<view class="pt-3"><text class="font-w">备案号</text>：{{mainData.record_no?mainData.record_no:''}}</view>
			<view class="pt-3"><text class="font-w">收款机构</text>：{{mainData.collection?mainData.collection:''}}</view>
			<view class="pt-3"><text class="font-w">执行机构</text>：{{mainData.execution?mainData.execution:''}}</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="flex px-25 py-3">
			<image @click="chooseThis" :src="isChoose?'../../static/images/toapplyfor-icon1.png':'../../static/images/toapplyfor-icon.png'" class="wh30 mr-1"></image>
			
			<view class="font-24 color8">
				你已阅读并同意 <text class="colorM"
				@click="Router.navigateTo({route:{path:'/pages/agreement/agreement?type=0'}})">《捐款须知》</text>
			</view>
		</view>
		
		
		<!-- 捐款按钮 -->
		<view style="height: 100rpx;"></view>
		<view class="shadowT bg-white flex1 py-1 px-25 p-fX bottom-0" v-if="mainData.progress_status==0">
			<view class="flex">
				<button class="flex4 font-24 line-h px-25" open-type="share">
					<image src="../../static/images/details-icon3.png" class="wh34 mb-1"></image>
					<view>分享</view>
				</button>
				<view class="flex4 font-24 line-h px-25"
				@click="Router.navigateTo({route:{path:'/pages/donateTogether/donateTogether?id='+mainData.id}})">
					<image src="../../static/images/details-icon2.png" class="wh34 mb-1"></image>
					<view>一起捐</view>
				</view>
			</view>
			<view class="btn80 Mgb colorf nn1" @click="Show(1)">立即捐款</view>
		</view>
		
		<!-- 结束按钮 -->
		<view class="shadowT bg-white flex1 py-1 px-25 p-fX bottom-0" v-if="mainData.progress_status==1">
			<view class="btn80 nn2 colorf w-100">捐款已结束</view>
		</view>
		
		<!-- 分享弹框 -->
		<view class="bg-mask line-h" v-show="share_show">
			<view class="mask p-aX bottom-0 radius20-T px-25 py-4">
				<view class="font-30 font-w pb-5 mb-1">分享</view>
				<view class="flex ">
					<button open-type="share" class="flex4 w-50">
						<image src="../../static/images/details-icon5.png" class="wx"></image>
						<view class="py-2">分享给好友</view>
					</button>
					<view class="flex4 w-50">
						<image src="../../static/images/details-icon4.png" class="wh80"></image>
						<view class="py-2">分享到朋友圈</view>
					</view>
				</view>
				
				<view class="py-4 px-3 p-a right-0 top-0"  @click="Show(0)">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh24"></image>
				</view>
			</view>
		</view>
		
		<!-- 捐款弹框 -->
		<view class="bg-mask" v-show="jk_show">
			<view class="mask p-aX bottom-0">
				<view class="font-30 font-w pb-5 mb-1 py-4 mx-25">捐款金额</view>
				<view class="jk mx-25">
					<view class="flex flex-wrap">
						<view class="jkItem p-r" @click="change(index)" v-for="(item,index) in priceData" :key="index" :class="jkIndex==index?'borderM':'b-e1'">
							<view>{{item}}</view>
							<image src="../../static/images/details-icon6.png" 
							class="wh34 p-a bottom-0 right-0" v-show="jkIndex==index"></image>
						</view>
					</view>
					<view class="flex b-e1 line-h px-3 py-2 font-30">
						<input type="digit" v-model="price" @focus="change(-1)" placeholder="自定义金额" />
						<view>元</view>
					</view>
				</view>
				<button class="btn-100 Mgb colorf w-100" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)"
				>确定</button>
				
				<view class="py-4 px-3 p-a right-0 top-0"  @click="Show(1)">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh24"></image>
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
				Utils:this.$Utils,
				open:false,
				share_show:false,
				jkIndex:0,
				jk_show:false,
				mainData:{},
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				},
				totalTeam:0,
				teamData:[],
				orderData:[],
				totalOrder:0,
				priceData:[10,20,50,100],
				price:'',
				isChoose:false,
				contentHeight:0,
				//realHeight:0,
		
				showHeight:0
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id
			self.price = self.priceData[self.jkIndex];
			const callback = (res) => {
				self.$Utils.loadAll(['getMainData'], self);
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
		},
		
		onReady () {
			const self = this;
		    setTimeout(() => {
				let query = wx.createSelectorQuery();		
				query.select('.content').boundingClientRect(rect=>{
					
					
					this.contentHeight = rect.height
					console.log(this.contentHeight);
					if(this.contentHeight>200){
						this.showHeight = 200
					}else{
						this.showHeight = this.contentHeight+21
					}
				},this).exec();
		    }, 300,this)
		},
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: '邀您和我一起关注['+self.mainData.title+']',
					path: '/pages/detail/detail?id=' + self.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData && self.mainData.mainImg && self.mainData.mainImg[0] && self.mainData.mainImg[0].url ?
						self.mainData.mainImg[0].url : '',
				}
			} else {
				return {
					title: '邀您和我一起关注['+self.mainData.title+']',
					path: '/pages/detail/detail?id=' + self.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData && self.mainData.mainImg && self.mainData.mainImg[0] && self.mainData.mainImg[0].url ?
						self.mainData.mainImg[0].url : '',
				}
			}
		},
		methods: {
			
			chooseThis(){
				const self = this;
				self.isChoose = !self.isChoose
			},
			
			nextPage(){
				const self = this;
				if(self.paginate.currentPage<self.page){
					self.paginate.currentPage++;
					self.getMainData(true)
				}
			},
			
			change(index){
				const self = this;
				self.jkIndex = index;
				if(index>=0){
					self.price = self.priceData[index]
				}else{
					self.price = ''
				}
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				
				if(self.price==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('金额错误', 'none');
					return
				};
				const callback = (user, res) => {
					console.log(res)
					self.addOrder();
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder() {
				const self = this;
				uni.showLoading({
					title:'支付中'
				});
				
				const postData = {};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					price: parseFloat(self.price).toFixed(2),
					type: 1,
					level:1,
					project_id:self.id
				};
				postData.refreshToken = true;
				console.log('post',postData)
				//return
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},
			
			
			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.score = {
					price: parseFloat(self.price).toFixed(2),
				};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					uni.hideLoading();
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									setTimeout(function() {
										self.jk_show = false
										self.Router.navigateTo({route:{path:'/pages/payEnd/payEnd?id='+self.orderId}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast(res.msg, 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							setTimeout(function() {
								self.jk_show = false
								self.Router.navigateTo({route:{path:'/pages/payEnd/payEnd?id='+self.orderId}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			getMainData(isNew) {
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
							user_type:0
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
					process: {
						tableName: 'Process',
						searchItem: {
							status:1,
						},
						middleKey: 'id',
						key: 'project_id',
						condition: 'in',
						
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						uni.setStorageSync('process',self.mainData.process);
						self.getTeamData()
						self.getOrderData()
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.projectGet(postData, callback);
			},
			
			getTeamData(isNew) {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = {
					count: 0,
					currentPage: 1,
					pagesize: 4,
					is_page: true,
				};
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
						self.teamData = res.info.data
					};
					self.totalTeam = res.info.total
				};
				self.$apis.teamGet(postData, callback);
			},
			
			getOrderData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate)
				postData.searchItem = {
					project_id:self.id,
					pay_status:1,
					user_type:0,
					type:1
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.orderData = res.info.data
					};
					self.totalOrder = res.info.total;
					self.page =Math.ceil(self.totalOrder/10);
				};
				self.$apis.orderGet(postData, callback);
			},
			
			openShow(){
				const self = this;
				self.open = !self.open;
				if(self.open){
					self.showHeight = self.contentHeight
				}else{
					self.showHeight = 200
				}
			},
			
			Show(type){
				const self = this;
				if(type==0){
					self.share_show = !self.share_show;
				}else{
					if(!self.jk_show){
						if(!self.isChoose){
							self.$Utils.showToast('请阅读并同意捐款须知','none')
							uni.setStorageSync('canClick', true);
							return
						};
					};
					self.jk_show = !self.jk_show;
				}
			}
			
		}
	}
</script>

<style scoped>
.content{overflow: hidden;}
.content image{margin-top: 20rpx;}

.btn50{width: 180rpx;}
.zk{box-shadow: 0 -10rpx 15rpx 10rpx rgba(255,255,255,1);}

.img{width: 155rpx;height: 155rpx;margin-right: 15rpx;margin-top: 20rpx;}
.bL{border-left: 1px solid #FFD9B7;}
.wh16{width: 16rpx;height: 16rpx;top: 0;left: -8rpx;}
.ee{top: 50%;margin-top: -8rpx;}
.name{max-width: 150rpx;display: block;}

.nn1{width: 400rpx;margin: 0;}
.nn2{background-color: #cacaca;}

.wx{width: 98rpx;height: 79rpx;}

.jk{height: 420rpx;}
.jkItem{width: 160rpx;line-height: 80rpx;text-align: center;margin: 0 21rpx 20rpx 0;}
.jkItem:nth-child(4n){margin-right: 0;}
input{flex: 1;font-size: 30rpx;}
</style>
