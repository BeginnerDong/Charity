<template>
	<view>
		
		<view class="p-r">
			<image src="../../static/images/posters-img.png" class="img"></image>
			
		</view>
		
		<view class="line-h flex4 con">
			<image src="../../static/images/my-img.png" class="wh100"></image>
			<view class="font-w pt-2">阿萨姆</view>
			<view class="font-24 py-3">邀你一起</view>
			<view class="font-26 font-w">“每个人做一点点，世界就会改变很多”</view>
			
			<view class="font-50 font-w pt-5 pb-2">0</view>
			<view class="font-24 color8">已筹到(元)</view>
		</view>
		
		<view class="shadowM my-5 px-25 py-4 flex">
			<image src="../../static/images/posters-img2.png" class="img1"></image>
			<view class="flex5 h120 flex-1 pl-2">
				<view class="font-w">用爱助力脱白之旅</view>
				<view class="font-24 color6 flex-1">陕西省慈善协会</view>
				<view class="flex color9 font-22">
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
						<view class="jkItem p-r" v-for="(item,index) in 4" :key="index" :class="jkIndex==index?'borderM':'b-e1'">
							<view>10</view>
							<image src="../../static/images/details-icon6.png" 
							class="wh34 p-a bottom-0 right-0" v-show="jkIndex==index"></image>
						</view>
					</view>
					<view class="flex b-e1 line-h px-3 py-2 font-30">
						<input type="text" value="" placeholder="自定义金额" />
						<view>元</view>
					</view>
				</view>
				<view class="btn-100 Mgb colorf w-100"
				@click="Router.navigateTo({route:{path:'/pages/payEnd/payEnd'}})">确定</view>
				
				<view class="py-4 px-3 p-a right-0 top-0"  @click="Show(0)">
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
				type:0,
				public_show:false,
				jk_show:false,
				share_show:false
			}
		},
		onLoad(options) {
			const self = this;
			if(options.type){
				self.type = options.type;
			}
		},
		methods: {
			
			Show(type){
				const self = this;
				if(type==0){
					self.jk_show = !self.jk_show;
				}else if(type==1){
					self.public_show = !self.public_show;
				}else{
					self.share_show = !self.share_show;
					self.public_show = false;
				}
			}
			
		}
	}
</script>

<style scoped>
.img{height: 440rpx;}
.con{margin-top: -50rpx;}
.img1{width: 160rpx;height: 120rpx;border-radius: 10rpx;}
.h120{height: 120rpx;}
.tt .btn80,.btn80-M{width: 340rpx;}
.btn80-M{border: 1px solid #D5D5D5;}

.mm,.pp{width: 660rpx;}
.maskBox{width: 660rpx;max-height: 80%;overflow-y: auto;}
.txt{min-height: 80rpx;}
.mm .btn80{width: 310rpx;margin: 0;}

.jk{height: 420rpx;}
.jkItem{width: 160rpx;line-height: 80rpx;text-align: center;margin: 0 22rpx 20rpx 0;}
.jkItem:nth-child(4n){margin-right: 0;}
input{flex: 1;font-size: 30rpx;}
</style>
