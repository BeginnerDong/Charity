<template>
	<view>
		
		<view class="Mgb" :style="{paddingTop:statusBar+'px'}">
			<view class="line-h colorf font-32 px-2 py-3">
				<view class="pb-3">陕西省中华职业教育社温暖工程办公室</view>
				<view class="letter">陕西省爱心温暖助学协会</view>
			</view>
		</view>
		
		<!-- banner -->
		<container>
			<ls-swiper :list="sliderData" imgKey="imgUrl" :loop="true" :dots='true' :autoplay='true' @clickItem="clickItem()" height='200' imgWidth='375' interval='3000' bottom='50'/>
		</container>
		
		<view class="bg-white mx-25 px-3 py-4 p-r radius10 card">
			<image src="../../static/images/homr-img2.png" class="logo mx-a"></image>
			
			<view class="flex2 line-h pt-5">
				<view class="flex4">
					<view class="font-36 font-w">{{allMoney}}</view>
					<view class="font-26 color6 pt-3">筹款金额/元</view>
				</view>
				<view class="flex4">
					<view class="font-36 font-w">{{people}}</view>
					<view class="font-26 color6 pt-3">支持人次</view>
				</view>
				<view class="flex4">
					<view class="font-36 font-w">{{num}}</view>
					<view class="font-26 color6 pt-3">在筹项目/个</view>
				</view>
			</view>
		</view>
		
		<view class="flexX py-4 px-25">
			<view class="sign" :class="signIndex==-1?'signOn':''" @click="changeSign(-1)"
			>全部</view>
			<view class="sign" :class="signIndex==index?'signOn':''" @click="changeSign(index)"
			v-for="(item,index) in labelData" :key="index">{{item.title}}</view>
		</view>
		
		<view class="mx-25 mb-3 line-h" v-for="(item,index) in mainData" :key="index"
		:data-id ="item.id"
		@click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+$event.currentTarget.dataset.id}})">
			<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="img radius10"></image>
			<view class="mx-3 bg-white radius10 shadow p-3 p-r txt">
				<view class="font-32 font-w avoidOverflow2">{{item.title?item.title:''}}</view>
				<view class="font-22 pt-4">已筹款金额(元)： <text class="colorR font-30 font-w">{{item.order?Utils.fmoney(item.order.count,2):0}}</text></view>
			</view>
		</view>
		 
		<view class="bg-f5 flex4 py-4 font-24 color9 line-h-lg">
			<image src="../../static/images/homr-img2.png" class="logo mx-a mb-3"></image>
			<view>统一社会信用代码：{{scope}}</view>
			<view>投诉与建议：{{phone}}</view>
			<view>由陕西省爱心学会提供运营支持</view>
		</view>
		
		
		<view style="height: 100rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
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
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar4.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import container from '../../components/container/index.vue'
	import LsSwiper from '../../components/ls-swiper/index.vue'
	const app = getApp();
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				is_show: false,
				swiperList: [{
					type: 'image',
					url: '../../static/images/home-banner.png'
				}, {
					type: 'image',
					url: '../../static/images/home-banner.png',
				}],
				statusBar: app.globalData.statusBar,
				signIndex:-1,
				sliderData:[],
				labelData:[],
				mainData:[],
				searchItem:{thirdapp_id:2},
				allMoney:0,
				people:0,
				num:0,
				scope:'',
				phone:''
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var callback = function(res) {
				self.$Utils.loadAll(['getSliderData','getLabelData','getMainData','getOrderData'], self);
				self.scope = uni.getStorageSync('user_info').thirdApp.scope
				self.phone = uni.getStorageSync('user_info').thirdApp.phone
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true,
			})
		},
		components: {
			container,
			LsSwiper
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
			
			getOrderData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					pay_status:1,
					user_type:0,
					type:1
				};
				postData.compute = {
					allPrice: [
						'sum',
						'price',
						{user_type:0,pay_status:1,type:1}
					],
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.allMoney =self.$Utils.fmoney(res.info.compute.allPrice,2);
						
					};
					self.people = res.info.total
					self.$Utils.finishFunc('getOrderData');
				};
				self.$apis.orderGet(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					order: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Order',
						searchItem: {
							status:1,
							pay_status:1,
							user_type:0,
							type:1
						},
						middleKey: 'id',
						key: 'project_id',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'price',
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
					if(!self.searchItem.label_id){
						self.num = res.info.total;
					}
					
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.projectGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 2
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id: 2
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						for (var i = 0; i < res.info.data.length; i++) {
							self.sliderData.push(res.info.data[i])
						}
					}
					console.log('self.sliderData',self.sliderData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			
			
			changeSign(index){
				const self = this;
				if(index!=self.signIndex){
					self.signIndex = index;
					if(self.signIndex>=0){
						self.searchItem.label_id = self.labelData[self.signIndex].id
					}else{
						delete self.searchItem.label_id
					};
					self.getMainData(true)
				}
				
			}
			
		}
	};
</script>

<style>
.letter{letter-spacing: 19rpx;}
.card{box-shadow: 0 0 20rpx 0 rgba(0,0,0,0.16);margin-top: -80rpx;}
.logo{width: 255rpx;height: 67rpx;}

.sign{flex-shrink: 0;}
.signOn{background-color: #FFECDA;color: #F76D0E;margin-right: 25rpx;}

.img{width: 100%;height: 320rpx;}
.txt{margin-top: -78rpx;}
</style>