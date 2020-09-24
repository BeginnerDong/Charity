<template>
	<view>
		
		<view class="px-25 py-3">
			<view class="pl-2 pb-5 bL" v-for="(item,index) in 3" :key="index">
				<view class="font-26 color6 p-r pb-1">
					<view>2020-08-12发布</view>
					<image src="../../static/images/details-icon.png" class="wh16 p-a"></image>
				</view>
				<view>截止目前，项目累计筹款25623.23元，机构于6月19日-8月2日共拨款付至25位患者123万元用于治疗。项目累计支出345万万元。感谢大家对贫困大病患者的关注和支持。</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="img"></image>
					<image src="../../static/images/details-img1.png" class="img"></image>
					<image src="../../static/images/details-img1.png" class="img"></image>
					<image src="../../static/images/details-img1.png" class="img"></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.img{width: 155rpx;height: 155rpx;margin-right: 15rpx;margin-top: 20rpx;}
.bL{border-left: 1px solid #FFD9B7;}
.wh16{width: 16rpx;height: 16rpx;top: 50%;left: -28rpx;margin-top: -12rpx;}
</style>
