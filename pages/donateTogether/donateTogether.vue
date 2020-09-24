<template>
	<view>
		
		<view>
			<image src="../../static/images/posters-img.png" class="img"></image>
		</view>
		
		<view class="line-h flex4 con">
			<image src="../../static/images/my-img.png" class="wh100"></image>
			<view class="font-w pt-2">阿萨姆</view>
		</view>
		
		<view class="px-25 mt-5">
			<view class="font-36 font-w">捐赠项目</view>
			<view class="py-3 flex">
				<image src="../../static/images/posters-img2.png" class="img1"></image>
				<view class="flex-1 pl-2 line-h">
					<view class="pb-3">用爱助力脱白之旅</view>
					<view class="font-24 color6 flex-1">陕西省慈善协会</view>
				</view>
			</view>
		</view>
		
		<view class="px-25 line-h">
			<view class="font-36 mt-5 font-w pb-2">发起者</view>
			<input type="text" value="" />
			<view class="font-36 mt-5 font-w pb-2">发起说明</view>
			<input type="text" value="每个人做一点点,世界就会改变很多" placeholder="" />
			<view class="font-32 mt-5 font-w pb-2">限定每笔捐款金额（元）</view>
			<input type="text" value="" placeholder="不限" />
			<view class="font-32 mt-5 font-w pb-2">设定筹款目标（元）</view>
			<input type="text" value="" placeholder="不限" />
		</view>
		
		<view class="py-5 mt-5 px-25 font-24 flex">
			<!-- <image src="../../static/images/toapplyfor-icon.png" class="wh32 mr-1"></image> -->
			<image src="../../static/images/toapplyfor-icon1.png" class="wh32 mr-1"></image>
			<view class="color9">同意 <text class="color2"
			@click="Router.navigateTo({route:{path:'/pages/agreement/agreement?type=1'}})">《用户协议》</text></view>
		</view>
		
		
		<view style="height: 120rpx;"></view>
		<view class="p-2 p-fX bottom-0 shadowT bg-white z10">
			<view class="btn80 Mgb colorf w-100"
			@click="Router.navigateTo({route:{path:'/pages/together/together?type=1'}})">预览</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.img{height: 440rpx;}
.con{margin-top: -50rpx;}
.img1{width: 160rpx;height: 120rpx;border-radius: 10rpx;}
.h120{height: 120rpx;}

input{width: 100%;height: 66rpx;font-size: 24rpx;border-radius: 10rpx;border: 1px solid #DDDDDD;padding: 0 20rpx;box-sizing: border-box;}
</style>
