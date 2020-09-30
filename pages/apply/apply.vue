<template>
	<view>
		
		<view class="list shadowM">
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">个人申请</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">项目申请</view>
		</view>
		
		<view class="px-2" v-show="liCurr==1">
			
			<view class="py-4 bB-e1 flex1">
				<view>姓名</view>
				<input type="text" v-model="submitData.name" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<picker mode="selector" :range="ageData" @change="changeAge">
				<view class="py-4 bB-e1 flex1">
					<view>年龄</view>
					<view class="font-24 color9 flex-1 text-r">{{submitData.age?submitData.age:'请选择'}}</view>
					<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
				</view>
			</picker>
			<view class="py-4 bB-e1 flex1">
				<view>姓名</view>
				<view class="font-26 flex">
					<view class="flex" @click="changeSex(1)">
						<image :src="submitData.sex==1?'../../static/images/toapplyfor-icon1.png':'../../static/images/toapplyfor-icon.png'" class="wh32 mx-1"></image>
						<view>男</view>
					</view>
					<view class="flex pl-5"  @click="changeSex(0)">
						<image :src="submitData.sex==0?'../../static/images/toapplyfor-icon1.png':'../../static/images/toapplyfor-icon.png'" class="wh32 mx-1"></image>
						<view>女</view>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>联系电话</view>
				<input type="number" maxlength="11" v-model="submitData.phone" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>身份证号</view>
				<input type="idcard"  v-model="submitData.card_no" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1 flex1">
				<view>家庭住址</view>
				<input type="text" v-model="submitData.address" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<picker mode="selector" :range="labelData" @change="chageLabel" :range-key="title">
				<view class="py-4 bB-e1 flex1">
					<view>项目类别</view>
					<view class="font-24 color9 flex-1 text-r">{{labelTitle?labelTitle:'请选择'}}</view>
					<image src="../../static/images/my-icon6.png" class="R-icon ml-1"></image>
				</view>
			</picker>
			<view class="py-4 bB-e1">
				<view>现状详情</view>
				<textarea type="text" v-model="submitData.details" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1">
				<view>求助</view>
				<textarea type="text" v-model="submitData.content" placeholder="请输入" placeholder-style="color:#999" />
			</view>
			<view class="py-4 bB-e1">
				<view>上传证明材料</view>
				<view class="flex flex-wrap">
					<image v-if="submitData.mainImg.length>0" :src="submitData.mainImg&&submitData.mainImg[0]?submitData.mainImg[0].url:''" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2" @click="upLoadImg('mainImg')" v-if="submitData.mainImg.length==0">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			<view class="py-4 bB-e1">
				<view>身份证与本人合照</view>
				<view class="flex flex-wrap">
					<image v-if="submitData.identityImg.length>0" :src="submitData.identityImg&&submitData.identityImg[0]?submitData.identityImg[0].url:''" class="wh200 mt-3 mr-2"></image>
					<view class="wh200 flex0 bg-f5 mt-3 mr-2"  @click="upLoadImg('identityImg')" v-if="submitData.identityImg.length==0">
						<image src="../../static/images/toapplyfor-icon2.png" class="sc"></image>
					</view>
				</view>
			</view>
			
		</view>
		
		
		<view class="px-2" v-show="liCurr==2">
			
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
			
			<view class="btn80 Mgb colorf w-100" @click="Utils.stopMultiClick(submit)">确定</view>
		</view>
		
		
		<!-- 提交成功弹框 -->
		<view class="bg-mask flex0" v-show="is_show">
			<view class="mask radius10 flex4 py-5 p-r">
				<image src="../../static/images/toapplyfor-icon3.png" class="icon mt-4"></image>
				<view class="font-30 color9 py-4">申请提交成功</view>
				
				<view class="p-2 p-a top-0 right-0" @click="showClose">
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
				Utils:this.$Utils,
				liCurr:1,
				is_show:false,
				submitData:{
					name:'',
					age:'',
					sex:1,
					phone:'',
					card_no:'',
					address:'',
					label_id:'',
					details:'',
					content:'',
					mainImg:[],
					identityImg:[],
					type:1
				},
				ageData:[],
				labelData:[],
				labelTitle:''
			}
		},
		onLoad() {
			const self = this;
			for(var i=18;i<100;i++){
				self.ageData.push(i+'岁');
			};
			self.$Utils.loadAll(['getLabelData'], self);
		},
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.liCurr==1){
					var pass = self.$Utils.checkComplete(self.submitData)
				}else{
					var pass = self.$Utils.checkComplete(self.submitDataTwo)
				};
				if(pass){
					self.applyAdd();
				}else{
					self.$Utils.showToast('请填写完整信息','none')
					uni.setStorageSync('canClick', true);
				}	
			},
				
			applyAdd(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(self.liCurr==1){
					postData.data = self.$Utils.cloneForm(self.submitData);
				}else{
					postData.data = self.$Utils.cloneForm(self.submitDataTwo);
				};
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code==100000) {
						self.Show()
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.applyAdd(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						uni.hideLoading();
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						var tempFilePaths = res.tempFilePaths;
						console.log('res11',res)
						for (var i = 0; i < tempFilePaths.length; i++) {
							var file = res.tempFiles[i];
							console.log('res.tempFiles[i].path',res.tempFiles[i].path)
							var obj = res.tempFiles[i].path.lastIndexOf(".");
							var ext = 'jpg';
							console.log(callback)
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:''
							}, callback)
						}
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 2
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			changeSex(e){
				const self = this;
				self.submitData.sex = e
			},
			
			changeAge(e){
				const self = this;
				self.submitData.age = self.ageData[e.detail.value]
			},
			
			chageLabel(e){
				const self = this;
				self.submitData.label_id = self.labelData[e.detail.value].id;
				self.labelTitle = self.labelData[e.detail.value].title;
			},
			
			changeLi(i){
				const self = this;
				self.liCurr = i
			},
			
			Show(){
				const self = this;
				self.is_show = true;
			},
			
			showClose(){
				const self = this;
				self.is_show = false;
				if(self.liCurr==1){
					self.submitData = {
						name:'',
						age:'',
						sex:1,
						phone:'',
						card_no:'',
						address:'',
						label_id:'',
						details:'',
						content:'',
						mainImg:[],
						identityImg:[],
						type:1
					}
				};
			},
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
