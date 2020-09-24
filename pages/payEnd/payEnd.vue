<template>
	<view>
		
		<view class="mt-5 mx-25 p-r">
			<image src="../../static/images/compaete-img.png" mode="widthFix"></image>
			<view class="p-aXY d-flex flex-column a-center pt-5">
				<image src="../../static/images/my-img.png" class="wh110"></image>
				<view class="font-44 font-w colorR pt-2">0.00元</view>
				<view class="pt-5 mt-3">感谢您此次募捐</view>
			</view>
		</view>
		
		<view class="btn80 Mgb colorf"
		@click="Router.navigateTo({route:{path:'/pages/certificate/certificate'}})">查看捐款证书</view>
		
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
.btn80{width: 500rpx;margin-top: 60rpx;}
</style>
