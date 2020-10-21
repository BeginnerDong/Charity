<template>
	<view>
		<!-- 个人 -->
		<view class="px-2" v-show="mainData.type==1">
			
			<view class="py-4 bB-e1 flex1">
				<view>姓名</view>
				<input type="text" v-model="mainData.name" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>年龄</view>
				<input type="text" v-model="mainData.age" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>性别</view>
				<input type="text" :value="mainData.sex==1?'男':'女'" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="text" v-model="mainData.phone" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>身份证号</view>
				<input type="text" v-model="mainData.card_no" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>家庭住址</view>
				<input type="text" v-model="mainData.address" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>项目类型</view>
				<input type="text" v-model="mainData.label.title" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>现状详情</view>
				<textarea type="text" v-model="mainData.details" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>求助</view>
				<textarea type="text" v-model="mainData.content" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>上传证明材料</view>
				<view class="flex flex-wrap">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>身份证与本人合照</view>
				<view class="flex flex-wrap">
					<image :src="mainData.identityImg&&mainData.identityImg[0]?mainData.identityImg[0].url:''" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			
		</view>
		
		<!-- 项目 -->
		<view class="px-2" v-show="mainData.type==2">
			
			<view class="py-4 bB-e1 flex1">
				<view>项目名称</view>
				<input type="text" v-model="mainData.title" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>项目地点</view>
				<input type="text" v-model="mainData.address" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系人</view>
				<input type="text" v-model="mainData.name" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="text" v-model="mainData.phone" disabled />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>项目类型</view>
				<input type="text" v-model="mainData.label.title" disabled />
			</view>
			
			<view class="py-4 bB-e1">
				<view>现状详情</view>
				<textarea type="text" :value="mainData.details" disabled />
			</view>
			<view class="py-4 bB-e1">
				<view>实施方案</view>
				<view class="flex flex-wrap">
					<image :src="mainData.programme&&mainData.programme[0]?mainData.programme[0].url:''" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>上传营业执照</view>
				<view class="flex flex-wrap">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="wh200 mt-3 mr-2"></image>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>法人与本人身份证合照</view>
				<view class="flex flex-wrap">
					<image :src="mainData.identityImg&&mainData.identityImg[0]?mainData.identityImg[0].url:''" class="wh200 mt-3 mr-2"></image>
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
				
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2,
					id:self.id
				};
				postData.getAfter = {
					label:{
						tableName:'Label',
						middleKey:'label_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title']
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.applyGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.li{width: 50%;}
input{font-size: 24rpx;flex: 1;text-align: right;}
textarea{font-size: 24rpx;background-color: #f5f5f5;padding: 30rpx;width: 100%;box-sizing: border-box;margin-top: 30rpx;}

.mask{width: 500rpx;}
.icon{width: 142rpx;height: 147rpx;}
</style>
