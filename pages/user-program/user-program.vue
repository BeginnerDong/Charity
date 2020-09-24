<template>
	<view class="px-25 bg-white">
		
		<view class="py-3 bB-f5">
			<view class="flex">
				<image src="../../static/images/img.png" class="img radius10"></image>
				<view class="h160 flex5 flex-1 pl-2">
					<view class="font-32">守护孩子的童话梦</view>
					<view class="font-26 color9">已捐金额(元)： <text class="colorR">26538</text></view>
				</view>
			</view>
			<view class="font-24 color8 bg-f5 p-2 radius10 flex mt-2"
			@click="Router.navigateTo({route:{path:'/pages/certificate/certificate'}})">
				<view class="flex-1 line-h-lg">
					<view>捐款次数：1次</view>
					<view>捐款时间：2020.09.23 15:23</view>
					<view>证书编号：SBHGSE655959</view>
				</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
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
<style>page{background-color: #f5f5f5;}</style>
<style scoped>
.img{width: 220rpx;height: 160rpx;}
.h160{height: 160rpx;}
</style>
