<template>
	<view>
		
		<view class="px-25 py-4">
			<image src="../../static/images/certificate-img1.png" mode="widthFix"></image>
			<view class="p-aX d-flex flex-column a-center pt-5 line-h" style="top: 0;">
				<view class="t1">捐赠证书</view>
				<view class="font-24 pb-5">捐赠证书编号：{{mainData.passage1?mainData.passage1:''}}</view>
				<view class="font-w font-44 pb-5">{{mainData.nickname?mainData.nickname:''}}</view>
				<view class="font-34 pt-4 pb-3">你为[{{mainData.project&&mainData.project[0]?mainData.project[0].title:''}}]成功捐助了</view>
				<view class="font-50 font-w colorR pb-4">{{mainData.price?mainData.price:''}}元</view>
				<view class="pt-4">感谢您，让这个世界更温暖</view>
			</view>
		</view>
		
		<view class="px-25 pb-4 flex">
			<image :src="qrUrl" class="wh140 radius-5"></image>
			<view class="flex-1 p-2">
				<view>长按识别小程序</view>
				<view>查看项目</view>
			</view>
			<view class="btn80 Mgb colorf" @click="canvasImage()">保存图片</view>
		</view>
		
		<view class="pc-container px-25" style="position: fixed;left:9000px" v-show="canvasShow">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas"  :style="{width: width+'px',height:height+'px'}"></canvas>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				qrUrl: '',
				rpx: 0,
				width:0,
				height:0,
				canvasShow: true,
			}
		},
		onLoad(options) {
			const self = this;
			if(options.id){
				self.id = options.id;
			};
			wx.getSystemInfo({
				success: function(res) {
					self.rpx = res.windowWidth / 375;
					self.width = 350* self.rpx 
					self.height = 480*self.rpx
					console.log('self.width',self.width)
				},
			})
			self.$Utils.loadAll(['getMainData','getQrCode'], self);
		},
		methods: {
			
			canvasImage() {
				const self = this;
				self.isCom = false;
				uni.showLoading({
					title: '正在生成图片',
					mask: true
				})
				var myCanvas = uni.createCanvasContext('mycanvas', this);
				myCanvas.drawImage('../../static/images/certificate-img1.png', 0, 0, 350*self.rpx, 480*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.font = '40px Arial'
				myCanvas.fillStyle = '#A1773F';
				myCanvas.fillText('捐赠证书', 175 *self.rpx, 90*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.font = '12px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText('捐赠证书编号：'+self.mainData.passage1, 175 *self.rpx, 117*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.font = 'bold 22px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText(self.mainData.nickname, 175 *self.rpx, 164*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.font = '17px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText('你为['+self.mainData.project[0].title+']成功捐助了', 175 *self.rpx, 226*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.font = 'bold 25px Arial'
				myCanvas.fillStyle = '#EC1111';
				myCanvas.fillText(self.mainData.price+'元', 175 *self.rpx, 266*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.font = '14px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText('感谢您，让这个世界更温暖', 175 *self.rpx, 320*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				var qr = self.qrUrl;
				uni.getImageInfo({
					src: qr,
					complete(res)  {
						console.log('res',res)
						self.isCom = true;
						myCanvas.drawImage(res.path, 140*self.rpx,
							410*self.rpx, 70*self.rpx, 70*self.rpx);
							
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					},
					fail(res) {
						self.$Utils.showToast('绘制二维码失败'+res.errMsg,'none');	
						self.isCom = true;
					}
				});
				var interval = setInterval(function() {
					if (self.isCom) {
						clearInterval(interval)
						
						self.canvasToTempFilePath()
					}
				}, 1000);
			},
			
			canvasToTempFilePath(){
				//const self = this;
				uni.canvasToTempFilePath({
					canvasId: 'mycanvas',
					success: (res) => {
						console.log(res)
						this.imgurl = res.tempFilePath;
						this.savePoster()
						uni.hideLoading();
					},
				}, this);
			},
			
			
			
			savePoster() {
				let self = this
				//若二维码未加载完毕，加个动画提高用户体验
				wx.showToast({
					icon: 'loading',
					title: '正在下载图片',
					duration: 1000
				})
				//判断用户是否授权"保存到相册"
				wx.getSetting({
					success(res) {
						//没有权限，发起授权
						if (!res.authSetting['scope.writePhotosAlbum']) {
							wx.authorize({
								scope: 'scope.writePhotosAlbum',
								success() { //用户允许授权，保存图片到相册
									self.savePhoto();
								},
								fail() { //用户点击拒绝授权，跳转到设置页，引导用户授权
									console.log('fail')
									wx.showToast({
										icon: 'none',
										title: '请至小程序设置中开启相册权限',
										duration: 1000
									})
								}
							})
						} else { //用户已授权，保存到相册
							self.savePhoto()
						}
					}
				})
			},
			
			savePhoto() {
				let self = this
				console.log()
				wx.saveImageToPhotosAlbum({
					filePath: self.imgurl,
					success(res) {
						wx.showToast({
							title: '保存成功',
							icon: "success",
							duration: 1000
						})
						/* var myCanvas = uni.createCanvasContext('mycanvas', this);
						console.log(myCanvas.width)
						myCanvas.clearRect(0, 0, 345, 380);
						myCanvas.save()
						myCanvas.restore() */
					}
				})
			},
			
			getQrCode() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.id,
					page: 'pages/detail/detail',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.qrUrl = res.info.url;
					}
					self.$Utils.finishFunc('getQrCode');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				postData.getAfter = {
					project: {
						tableName: 'Project',
						searchItem: {
							status:1,
						},
						middleKey: 'project_id',
						key: 'id',
						condition: 'in',
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #F9F5EA;}
</style>
<style scoped>
.t1{font-size: 80rpx;padding: 90rpx 0 30rpx;color: #A1773F;}

.btn80{width: 300rpx;}
</style>
