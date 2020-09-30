<template>
	<view>
		
		<view class="px-25 py-3">
			<view class="pl-2 pb-5 bL" v-for="(item,index) in mainData" :key="index">
				<view class="font-26 color6 p-r pb-1">
					<view>{{item.create_time.substr(0,10)}}发布</view>
					<image src="../../static/images/details-icon.png" class="wh16 p-a"></image>
				</view>
				<view>
					<view class="content ql-editor" style="padding:0;" v-html="item.content">
					</view>
				</view>
				<view class="flex flex-wrap">
					<image v-for="(c_item,c_index) in item.mainImg" :key="c_index" :src="c_item.url" class="img"></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:[]
			}
		},
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('process')
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
