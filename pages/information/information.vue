<template>
	<view>
		
		<view class="mx-25 py-3 flex bB-f5" v-for="(item,index) in 3" :key="index"
		@click="Router.navigateTo({route:{path:'/pages/infoDetail/infoDetail'}})">
			<view class="flex-1 pr-3">
				<view class="font-32 avoidOverflow2">中华慈善总会开展2017年度“慈善情暖万家”活动</view>
				<view class="font-24 color9 pt-2">2020.07.31</view>
			</view>
			<image src="../../static/images/imformation-img.png" class="img radius10"></image>
		</view>
		
		
		
		<view style="height: 100rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
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
.img{width: 220rpx;height: 150rpx;}
</style>
