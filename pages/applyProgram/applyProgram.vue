<template>
	<view>
		<!-- 个人 -->
		<view class="px-2" v-show="type==0">
			
			<view class="py-4 bB-e1 flex1">
				<view>姓名</view>
				<input type="text" value="" disabled />
			</view>
			<picker mode="selector">
				<view class="py-4 bB-e1 flex1">
					<view>年龄</view>
					<view class="font-24 color9 flex-1 text-r">请选择</view>
					<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
				</view>
			</picker>
			<view class="py-4 bB-e1 flex1">
				<view>姓名</view>
				<view class="font-26 flex">
					<view class="flex">
						<image src="../../static/images/toapplyfor-icon.png" class="wh32 mx-1"></image>
						<view>男</view>
					</view>
					<view class="flex pl-5">
						<image src="../../static/images/toapplyfor-icon.png" class="wh32 mx-1"></image>
						<view>女</view>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>身份证号</view>
				<input type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>家庭住址</view>
				<input type="text" value="" disabled />
			</view>
			<picker mode="selector">
				<view class="py-4 bB-e1 flex1">
					<view>项目类别</view>
					<view class="font-24 color9 flex-1 text-r">请选择</view>
					<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
				</view>
			</picker>
			<view class="py-4 bB-e1">
				<view>现状详情</view>
				<textarea type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>求助</view>
				<textarea type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>上传证明材料</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>身份证与本人合照</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			
		</view>
		
		<!-- 项目 -->
		<view class="px-2" v-show="type==1">
			
			<view class="py-4 bB-e1 flex1">
				<view>项目名称</view>
				<input type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>项目地点</view>
				<input type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系人</view>
				<input type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="text" value="" disabled />
			</view>
			<picker mode="selector">
				<view class="py-4 bB-e1 flex1">
					<view>项目类型</view>
					<view class="font-24 color9 flex-1 text-r">请选择</view>
					<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
				</view>
			</picker>
			<view class="py-4 bB-e1">
				<view>现状详情</view>
				<textarea type="text" value="" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>实施方案</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>上传营业执照</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>法人与本人身份证合照</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
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
				type:1
			}
		},
		methods: {
			
			changeLi(i){
				const self = this;
				self.type = i
			}
			
		}
	}
</script>

<style scoped>
.li{width: 50%;}
input{font-size: 24rpx;flex: 1;text-align: right;}
textarea{font-size: 24rpx;background-color: #f5f5f5;padding: 30rpx;width: 100%;box-sizing: border-box;margin-top: 30rpx;height: auto;}

.mask{width: 500rpx;}
.icon{width: 142rpx;height: 147rpx;}
</style>
