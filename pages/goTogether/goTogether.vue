<template>
	<view>

		<view class="p-r">
			<image :src="mainData.project&&mainData.project[0]&&mainData.project[0].mainImg&&mainData.project[0].mainImg[0]?mainData.project[0].mainImg[0].url:''"
			 class="img"></image>

		</view>

		<view class="line-h flex4 con">
			<image :src="mainData.user&&mainData.user[0]?mainData.user[0].headImgUrl:''" class="wh100 overflow-h" style="border-radius: 50%;"></image>
			<view class="font-w pt-2">{{mainData.name?mainData.name:''}}</view>
			<view class="font-24 py-3">邀你一起</view>
			<view class="font-26 font-w">“{{mainData.explain?mainData.explain:''}}”</view>

			<view class="font-50 font-w pt-5 pb-2">{{mainData.order?Utils.fmoney(mainData.order.allPrice,2):0}}</view>
			<view class="font-24 color8">已筹到(元)</view>
		</view>

		<view class="shadowM my-5 px-25 py-4 flex">
			<image :src="mainData.project&&mainData.project[0]&&mainData.project[0].mainImg&&mainData.project[0].mainImg[0]?mainData.project[0].mainImg[0].url:''"
			 class="img1"></image>
			<view class="flex5 h120 flex-1 pl-2">
				<view class="font-w">{{mainData.project&&mainData.project[0]?mainData.project[0].title:''}}</view>
				<view class="font-24 color6 flex-1">{{mainData.execution?mainData.execution:''}}</view>
				<view class="flex color9 font-22" @click="open">
					<view>查看详情</view>
					<image src="../../static/images/details-icon7.png" class="B-icon ml-1"></image>
				</view>
			</view>
		</view>




		<view style="height: 120rpx;"></view>

		<!-- 非预览展示 -->
		<view class="p-2 p-fX bottom-0 shadowT bg-white">
			<view class="btn80 Mgb colorf w-100" @click="Show(0)">捐款支持</view>
			<!-- <view class="btn80 bg-c9 colorf w-100">捐款已结束</view> -->
		</view>



		<!-- 捐款弹框 -->
		<view class="bg-mask" v-show="jk_show">
			<view class="mask p-aX bottom-0">
				<view class="font-30 font-w pb-5 mb-1 py-4 mx-25">捐款金额</view>
				<view class="jk mx-25">
					<view class="flex flex-wrap">
						<view class="jkItem p-r" @click="change(index)" v-for="(item,index) in priceData" :key="index" :class="jkIndex==index?'borderM':'b-e1'">
							<view>{{item}}</view>
							<image src="../../static/images/details-icon6.png" class="wh34 p-a bottom-0 right-0" v-show="jkIndex==index"></image>
						</view>
					</view>
					<view class="flex b-e1 line-h px-3 py-2 font-30">
						<input type="digit" v-model="price" @focus="change(-1)" placeholder="自定义金额" />
						<view>元</view>
					</view>
				</view>
				<button class="btn-100 Mgb colorf w-100" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">确定</button>

				<view class="py-4 px-3 p-a right-0 top-0" @click="Show(0)">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh24"></image>
				</view>
			</view>
		</view>


		<uni-popup ref="popup" type="bottom">
			<view class="font-40 font-w">项目详情</view>
			<view class="ql-editor" style="padding:0;" v-html="mainData.project&&mainData.project[0]?mainData.project[0].content:''">
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import uniPopupMessage from '@/components/uni-popup/uni-popup-message.vue'
	import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue'
	export default {
		components: {
			uniPopup,
			uniPopupMessage,
			uniPopupDialog
		},
		data() {
			return {
				Router: this.$Router,
				type: 0,
				public_show: false,
				jk_show: false,
				share_show: false,
				mainData: {},
				Utils: this.$Utils,
				priceData: [10, 20, 50, 100],
				price: '',
				jkIndex: 0,

			}
		},
		onLoad(options) {
			const self = this;
			if (options.id) {
				self.id = options.id;
			}
			self.price = self.priceData[self.jkIndex];
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			open() {
				this.$refs.popup.open()
			},
			change(index) {
				const self = this;
				self.jkIndex = index;
				if (index >= 0) {
					self.price = self.priceData[index]
				} else {
					self.price = ''
				}
			},

			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.price == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('金额错误', 'none');
					return
				};
				if (parseFloat(self.price) < parseFloat(self.mainData.price)) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('限制捐款金额' + self.mainData.price + '元以上（含' + self.mainData.price + '元）', 'none');
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
					title: '支付中'
				});

				const postData = {};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					price: parseFloat(self.price).toFixed(2),
					type: 1,
					level: 1,
					project_id: self.mainData.project_id,
					team_id: self.id,
					relation_user: self.mainData.user_no
				};
				postData.refreshToken = true;
				console.log('post', postData)
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
										self.Router.navigateTo({
											route: {
												path: '/pages/payEnd/payEnd?id=' + self.orderId
											}
										})
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
								self.Router.navigateTo({
									route: {
										path: '/pages/payEnd/payEnd?id=' + self.orderId
									}
								})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},


			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
					user_type: 0
				};
				postData.getAfter = {
					project: {
						tableName: 'Project',
						searchItem: {
							status: 1,
						},
						middleKey: 'project_id',
						key: 'id',
						condition: 'in',
					},
					user: {
						tableName: 'User',
						searchItem: {
							status: 1,
							user_type: 0
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
					},
					order: {
						tableName: 'Order',
						searchItem: {
							status: 1,
							pay_status: 1,
							user_type: 0,
							type: 1
						},
						middleKey: 'id',
						key: 'team_id',
						condition: 'in',
						compute: {
							allPrice: [
								'sum',
								'price',
								{
									status: 1,
									pay_status: 1,
									user_type: 0,
									type: 1
								}
							],
							count: [
								'count',
								'count',
								{
									status: 1,
									pay_status: 1,
									user_type: 0,
									type: 1
								}
							]
						},
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.teamGet(postData, callback);
			},

			Show(type) {
				const self = this;
				if (type == 0) {
					self.jk_show = !self.jk_show;
				} else if (type == 1) {
					self.public_show = !self.public_show;
				} else {
					self.share_show = !self.share_show;
					self.public_show = false;
				}
			}

		}
	}
</script>

<style scoped>
	.img {
		height: 440rpx;
	}

	.con {
		margin-top: -50rpx;
	}

	.img1 {
		width: 160rpx;
		height: 120rpx;
		border-radius: 10rpx;
	}

	.h120 {
		height: 120rpx;
	}

	.tt .btn80,
	.btn80-M {
		width: 340rpx;
	}

	.btn80-M {
		border: 1px solid #D5D5D5;
	}

	.mm,
	.pp {
		width: 660rpx;
	}

	.maskBox {
		width: 660rpx;
		max-height: 80%;
		overflow-y: auto;
	}

	.txt {
		min-height: 80rpx;
	}

	.mm .btn80 {
		width: 310rpx;
		margin: 0;
	}

	.jk {
		height: 420rpx;
	}

	.jkItem {
		width: 160rpx;
		line-height: 80rpx;
		text-align: center;
		margin: 0 21rpx 20rpx 0;
	}

	.jkItem:nth-child(4n) {
		margin-right: 0;
	}

	input {
		flex: 1;
		font-size: 30rpx;
	}
	.popup-content {
		background-color: #fff;
		padding: 15px;
	}
</style>
