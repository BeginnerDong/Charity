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
		
		<view class="bg-white px-25">
			
			<view class="d-flex a-start mx-25 py-4 bB-f5" v-for="(item,index) in 3" :key="index">
				<image src="../../static/images/posters-img1.png" class="wh100 radius-5"></image>
				<view class="pt-1 pl-2 line-h d-flex j-sb a-start flex-1">
					<view>
						<view class="font-30 pb-2">开度多</view>
						<view class="font-26 pb-3">和18位小伙伴筹得123.32元</view>
						<view class="font-22 color9">每个人做一点点，世界就会改变很多</view>
					</view>
					<view class="font-22 color9">25天前</view>
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

<style>
page{background-color: #f5f5f5;}
</style>
