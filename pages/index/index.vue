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
			<ls-swiper :list="swiperList" imgKey="imgUrl" :loop="true" :dots='true' :autoplay='true' @clickItem="clickItem()" height='200' imgWidth='375' interval='3000' bottom='50'/>
		</container>
		
		<view class="bg-white mx-25 px-3 py-4 p-r radius10 card">
			<image src="../../static/images/homr-img2.png" class="logo mx-a"></image>
			
			<view class="flex2 line-h pt-5">
				<view class="flex4">
					<view class="font-36 font-w">8,985,252.23</view>
					<view class="font-26 color6 pt-3">筹款金额/元</view>
				</view>
				<view class="flex4">
					<view class="font-36 font-w">10,230</view>
					<view class="font-26 color6 pt-3">支持人次</view>
				</view>
				<view class="flex4">
					<view class="font-36 font-w">6</view>
					<view class="font-26 color6 pt-3">在筹项目/个</view>
				</view>
			</view>
		</view>
		
		<view class="flexX py-4 px-25">
			<view class="sign" :class="signIndex==index?'signOn':''" @click="changeSign(index)"
			v-for="(item,index) in 6" :key="index">教育助学</view>
		</view>
		
		<view class="mx-25 mb-3 line-h" v-for="v in 2" :key="v"
		@click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
			<image src="../../static/images/homr-img1.png" class="img radius10"></image>
			<view class="mx-3 bg-white radius10 shadow p-3 p-r txt">
				<view class="font-32 font-w avoidOverflow2">守护孩子的童话梦</view>
				<view class="font-22 pt-4">已筹款金额(元)： <text class="colorR font-30 font-w">26853</text></view>
			</view>
		</view>
		 
		<view class="bg-f5 flex4 py-4 font-24 color9 line-h-lg">
			<image src="../../static/images/homr-img2.png" class="logo mx-a mb-3"></image>
			<view>统一社会信用代码：2563886585699566SSSVSS</view>
			<view>投诉与建议：029-23514875</view>
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
				<view>咨询</view>
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
				is_show: false,
				swiperList: [{
				                type: 'image',
				                url: '../../static/images/home-banner.png'
				            }, {
				                type: 'image',
				                url: '../../static/images/home-banner.png',
				            }],
				statusBar: app.globalData.statusBar,
				signIndex:0
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		components: {
			container,
			LsSwiper
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			},
			
			changeSign(index){
				const self = this;
				self.signIndex = index;
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