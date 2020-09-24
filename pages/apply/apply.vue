<template>
	<view>
		
		<view class="list shadowM">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">个人申请</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">项目申请</view>
		</view>
		
		<view class="px-2" v-show="liCurr==0">
			
			<view class="py-4 bB-e1 flex1">
				<view>姓名</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
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
						<!-- <image src="../../static/images/toapplyfor-icon.png" class="wh32 mx-1"></image> -->
						<image src="../../static/images/toapplyfor-icon1.png" class="wh32 mx-1"></image>
						<view>男</view>
					</view>
					<view class="flex pl-5">
						<image src="../../static/images/toapplyfor-icon.png" class="wh32 mx-1"></image>
						<!-- <image src="../../static/images/toapplyfor-icon1.png" class="wh32 mx-1"></image> -->
						<view>女</view>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>身份证号</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>家庭住址</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
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
				<textarea type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1">
				<view>求助</view>
				<textarea type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1">
				<view>上传证明材料</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>身份证与本人合照</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			
		</view>
		
		
		<view class="px-2" v-show="liCurr==1">
			
			<view class="py-4 bB-e1 flex1">
				<view>项目名称</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>项目地点</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系人</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
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
				<textarea type="text" value="" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1">
				<view>实施方案</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>上传营业执照</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>法人与本人身份证合照</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			
		</view>
		
		
		<view class="px-25">
			<view class="flex py-3 mt-5">
				<image src="../../static/images/toapplyfor-icon1.png" class="wh30 mr-1"></image>
				<!-- <image src="../../static/images/toapplyfor-icon.png" class="wh30 mr-1"></image> -->
				<view class="font-24 color8">
					你已阅读并同意 <text class="colorM"
					@click="Router.navigateTo({route:{path:'/pages/agreement/agreement?type=1'}})">《用户须知》</text>
				</view>
			</view>
			
			<view class="btn80 Mgb colorf w-100" @click="Show">确定</view>
		</view>
		
		
		<!-- 提交成功弹框 -->
		<view class="bg-mask flex0" v-show="is_show">
			<view class="mask radius10 flex4 py-5 p-r">
				<image src="../../static/images/toapplyfor-icon3.png" class="icon mt-4"></image>
				<view class="font-30 color9 py-4"
				@click="Router.navigateTo({route:{path:'/pages/applyProgram/applyProgram'}})">申请提交成功</view>
				
				<view class="p-2 p-a top-0 right-0" @click="Show">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh20"></image>
				</view>
			</view>
		</view>
		
		
		
		<view style="height: 140rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/information/information'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>咨询</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar3-a.png" mode=""></image>
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
				Router:this.$Router,
				liCurr:1,
				is_show:false
			}
		},
		methods: {
			
			changeLi(i){
				const self = this;
				self.liCurr = i
			},
			
			Show(){
				const self = this;
				self.is_show = !self.is_show;
			}
			
		}
	}
</script>

<style scoped>
.li{width: 50%;}
input{font-size: 24rpx;flex: 1;text-align: right;}
textarea{font-size: 24rpx;background-color: #f5f5f5;padding: 30rpx;width: 100%;box-sizing: border-box;margin-top: 30rpx;min-height: 260rpx;height: auto;}
.sc{width: 68rpx;height: 58rpx;}

.mask{width: 500rpx;}
.icon{width: 142rpx;height: 147rpx;}
</style>
