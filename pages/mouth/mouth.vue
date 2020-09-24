<template>
	<view>
		
		<view class="Mgb py-4 colorf line-h flex">
			<view class="flex4 w-33 rb">
				<view class="font-36 font-w pb-2">9</view>
				<view class="font-22">发起一起捐/次</view>
			</view>
			<view class="flex4 w-33 rb">
				<view class="font-36 font-w pb-2">4</view>
				<view class="font-22">参与伙伴/次</view>
			</view>
			<view class="flex4 w-33">
				<view class="font-36 font-w pb-2">600</view>
				<view class="font-22">共筹善款/元</view>
			</view>
		</view>
		
		<view class="py-3 px-25 bB-f5">
			<view class="flex">
				<image src="../../static/images/img.png" class="img radius10"></image>
				<view class="h160 flex5 flex-1 pl-2">
					<view class="font-32">守护孩子的童话梦</view>
					<view class="font-26 color9">已捐金额(元)： <text class="colorR">26538</text></view>
				</view>
			</view>
			<view class="font-24 color8 bg-f5 p-2 radius10 flex mt-2" v-show="type==1"
			@click="Router.navigateTo({route:{path:'/pages/certificate/certificate'}})">
				<view class="flex-1 line-h-lg">
					<view>捐款次数：1次</view>
					<view>捐款时间：2020.09.23 15:23</view>
					<view>证书编号：SBHGSE655959</view>
				</view>
				<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
			</view>
			<view class="font-24 color8 bg-f5 p-2 radius10 flex mt-2" v-show="type==0">
				<view class="flex-1 line-h-lg">
					<view>捐款人数：15</view>
					<view>发起时间：2020.09.23</view>
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
				type:0
			}
		},
		onLoad(options) {
			const self = this;
			if(options.type){
				self.type = options.type;
			}
			if(options.type==0){
				self.changeTit('发起的一起捐')
			}else if(options.type==1){
				self.changeTit('月捐管理')
			}
		},
		methods: {
			
			changeTit(title){
				uni.setNavigationBarTitle({
				    title: title
				});
			}
			
		}
	}
</script>

<style scoped>
.img{width: 220rpx;height: 160rpx;}
.h160{height: 160rpx;}
</style>
