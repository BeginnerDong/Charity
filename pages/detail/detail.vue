<template>
	<view>
		
		<view class="px-2 py-3">
			<view class="font-44 pb-5 font-w mb-1">坚强小白寻觅生机</view>
			<view class="flex line-h">
				<view class="flex4 w-50 rb">
					<view class="font-50 colorR font-w">13599</view>
					<view class="font-22 pt-2">已经筹到(元)</view>
				</view>
				<view class="flex4 w-50">
					<view class="font-50 colorR font-w">10183</view>
					<view class="font-22 pt-2">捐款次数</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-3 p-r">
			<view class="font-34 line-h pl-2 title">求助人故事</view>
			<view class="font-30 py-2 content" :style="{height:open?'auto':'400rpx'}">
				我是谢双桥，我的父亲谢志杰，他今年65岁，我们一家人生活在湖北省十堰市竹溪县蒋家堰镇中原村八组。
				<image src="../../static/images/details-img.png" mode="widthFix"></image>
			</view>
			
			<view class="py-2 bg-white p-aX bottom-0" :class="open?'':'zk'">
				<view class="btn50 bg-f5 flex0" @click="openShow()">
					<image src="../../static/images/icon.png" class="wh20 mr-1" v-if="open"></image>
					<image src="../../static/images/icon1.png" class="wh20 mr-1" v-else></image>
					<view class="font-26">展开全文</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-25 py-4">
			<view class="font-34 line-h pl-2 title">进展反馈</view>
			<view class="mt-4 pl-2 p-r bL">
				<image src="../../static/images/details-icon.png" class="wh16 p-a"></image>
				<view class="font-26 color6">2020-08-12</view>
				<view>截止目前，项目累计筹款25623.23元，机构于6月19日-8月2日共拨款付至25位患者123万元用于治疗。项目累计支出345万万元。感谢大家对贫困大病患者的关注和支持。</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/details-img1.png" class="img"></image>
					<image src="../../static/images/details-img1.png" class="img"></image>
					<image src="../../static/images/details-img1.png" class="img"></image>
					<image src="../../static/images/details-img1.png" class="img"></image>
				</view>
			</view>
			<view class="pl-2 py-5 p-r line-h bL flex1"
			@click="Router.navigateTo({route:{path:'/pages/progress/progress'}})">
				<image src="../../static/images/details-icon.png" class="wh16 p-a ee"></image>
				<view>查看近期反馈</view>
				<image src="../../static/images/my-icon6.png" class="R-icon"></image>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		
		<view class="px-25 py-4">
			<view class="font-34 line-h pl-2 title">筹款记录</view>
			<view class="py-4 p-r line-h font-26 flex1"
			@click="Router.navigateTo({route:{path:'/pages/record/record'}})"> 
				<view>查看近期反馈</view>
				<image src="../../static/images/my-icon6.png" class="R-icon"></image>
			</view>
			<view class="flex1">
				<view class="flex4 font-26" v-for="(item,index) in 4" :key="index"
				@click="Router.navigateTo({route:{path:'/pages/together/together'}})">
					<image src="../../static/images/details-img2.png" class="wh100 radius-5 mb-1"></image>
					<view>猜猜我是谁</view>
					<view>携手3人</view>
				</view>
			</view>
			<view class="font-24 pt-4">
				<view class="flex py-1" v-for="(item,index) in 10" :key="index">
					<image src="../../static/images/details-img3.png" class="wh40 radius-5"></image>
					<view class="flex flex-1 px-1">
						<text class="name avoidOverflow">居心哈哈哈哈哈</text>捐出
						<text class="colorR">88</text>元
					</view>
					<view class="color9">6天前</view>
				</view>
			</view>
			<view class="font-24 color pt-2">换一批 ></view>
		</view>
		<view class="f5Bj-H20"></view>
		
		
		<view class="px-25 py-4 line-h">
			<view class="font-34 line-h pl-2 pb-1 title">执行机构信息</view>
			<view class="pt-3"><text class="font-w">备案号</text>：2659898945616546</view>
			<view class="pt-3"><text class="font-w">收款机构</text>：陕西省慈善办</view>
			<view class="pt-3"><text class="font-w">执行机构</text>：西安市雁塔区城西公益援助中心</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="flex px-25 py-3">
			<image src="../../static/images/toapplyfor-icon1.png" class="wh30 mr-1"></image>
			<!-- <image src="../../static/images/toapplyfor-icon.png" class="wh30 mr-1"></image> -->
			<view class="font-24 color8">
				你已阅读并同意 <text class="colorM"
				@click="Router.navigateTo({route:{path:'/pages/agreement/agreement?type=0'}})">《捐款须知》</text>
			</view>
		</view>
		
		
		<!-- 捐款按钮 -->
		<view style="height: 100rpx;"></view>
		<view class="shadowT bg-white flex1 py-1 px-25 p-fX bottom-0">
			<view class="flex">
				<view class="flex4 font-24 line-h px-25" @click="Show(0)">
					<image src="../../static/images/details-icon3.png" class="wh34 mb-1"></image>
					<view>分享</view>
				</view>
				<view class="flex4 font-24 line-h px-25"
				@click="Router.navigateTo({route:{path:'/pages/donateTogether/donateTogether'}})">
					<image src="../../static/images/details-icon2.png" class="wh34 mb-1"></image>
					<view>一起捐</view>
				</view>
			</view>
			<view class="btn80 Mgb colorf nn1" @click="Show(1)">立即捐款</view>
		</view>
		
		<!-- 结束按钮 -->
		<!-- <view class="shadowT bg-white flex1 py-1 px-25 p-fX bottom-0">
			<view class="btn80 nn2 colorf w-100">捐款已结束</view>
		</view> -->
		
		<!-- 分享弹框 -->
		<view class="bg-mask line-h" v-show="share_show">
			<view class="mask p-aX bottom-0 radius20-T px-25 py-4">
				<view class="font-30 font-w pb-5 mb-1">分享</view>
				<view class="flex ">
					<button open-type="share" class="flex4 w-50">
						<image src="../../static/images/details-icon5.png" class="wx"></image>
						<view class="py-2">分享给好友</view>
					</button>
					<view class="flex4 w-50">
						<image src="../../static/images/details-icon4.png" class="wh80"></image>
						<view class="py-2">分享到朋友圈</view>
					</view>
				</view>
				
				<view class="py-4 px-3 p-a right-0 top-0"  @click="Show(0)">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh24"></image>
				</view>
			</view>
		</view>
		
		<!-- 捐款弹框 -->
		<view class="bg-mask" v-show="jk_show">
			<view class="mask p-aX bottom-0">
				<view class="font-30 font-w pb-5 mb-1 py-4 mx-25">捐款金额</view>
				<view class="jk mx-25">
					<view class="flex flex-wrap">
						<view class="jkItem p-r" v-for="(item,index) in 4" :key="index" :class="jkIndex==index?'borderM':'b-e1'">
							<view>10</view>
							<image src="../../static/images/details-icon6.png" 
							class="wh34 p-a bottom-0 right-0" v-show="jkIndex==index"></image>
						</view>
					</view>
					<view class="flex b-e1 line-h px-3 py-2 font-30">
						<input type="text" value="" placeholder="自定义金额" />
						<view>元</view>
					</view>
				</view>
				<view class="btn-100 Mgb colorf w-100"
				@click="Router.navigateTo({route:{path:'/pages/payEnd/payEnd'}})">确定</view>
				
				<view class="py-4 px-3 p-a right-0 top-0"  @click="Show(1)">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh24"></image>
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
				open:false,
				share_show:false,
				jkIndex:0,
				jk_show:false
			}
		},
		methods: {
			
			openShow(){
				const self = this;
				self.open = !self.open;
			},
			
			Show(type){
				const self = this;
				if(type==0){
					self.share_show = !self.share_show;
				}else{
					self.jk_show = !self.jk_show;
				}
			}
			
		}
	}
</script>

<style scoped>
.content{overflow: hidden;}
.content image{margin-top: 20rpx;}

.btn50{width: 180rpx;}
.zk{box-shadow: 0 -10rpx 15rpx 10rpx rgba(255,255,255,1);}

.img{width: 155rpx;height: 155rpx;margin-right: 15rpx;margin-top: 20rpx;}
.bL{border-left: 1px solid #FFD9B7;}
.wh16{width: 16rpx;height: 16rpx;top: 0;left: -8rpx;}
.ee{top: 50%;margin-top: -8rpx;}
.name{max-width: 150rpx;display: block;}

.nn1{width: 400rpx;margin: 0;}
.nn2{background-color: #cacaca;}

.wx{width: 98rpx;height: 79rpx;}

.jk{height: 420rpx;}
.jkItem{width: 160rpx;line-height: 80rpx;text-align: center;margin: 0 22rpx 20rpx 0;}
.jkItem:nth-child(4n){margin-right: 0;}
input{flex: 1;font-size: 30rpx;}
</style>
