<template>
	<view>
		
		<view class="mx-25 mb-3 line-h p-r" v-for="v in 2" :key="v"
		@click="Router.navigateTo({route:{path:'/pages/applyProgram/applyProgram'}})">
			<image src="../../static/images/homr-img1.png" class="img radius10"></image>
			<view class="mx-3 bg-white radius10 shadow p-3 p-r txt">
				<view class="font-32 font-w avoidOverflow2">守护孩子的童话梦</view>
				<view class="font-22 pt-4">已筹款金额(元)： <text class="colorR font-30 font-w">26853</text></view>
			</view>
			<view class="o5 font-24 colorf p-a left-0 top-0 sign">个人·审核中</view>
			<!-- <view class="o5 font-24 colorf p-a left-0 top-0 sign">项目·进行中</view> -->
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
.img{width: 100%;height: 320rpx;}
.txt{margin-top: -78rpx;}
.sign{border-radius: 10rpx 0 10rpx 0;line-height: 46rpx;display: inline-block;padding: 0 10rpx;}
</style>
