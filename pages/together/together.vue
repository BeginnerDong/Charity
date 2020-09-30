<template>
	<view>
		
		<view class="p-r">
			<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="img"></image>
			<!-- 预览展示 -->
			<view class="o5 font-24 colorf line-h-big p-aX top-0 text-c">当前正在预览模式</view>
		</view>
		
		<view class="line-h flex4 con">
			<view  class="wh100 radius-5 overflow-h z10">
				<open-data type="userAvatarUrl"></open-data>
			</view>
			<view class="font-w pt-2">{{submitData.name?submitData.name:''}}</view>
			<view class="font-24 py-3">邀你一起</view>
			<view class="font-26 font-w">“{{submitData.explain?submitData.explain:''}}”</view>
			
			<view class="font-50 font-w pt-5 pb-2">0</view>
			<view class="font-24 color8">已筹到(元)</view>
		</view>
		
		<view class="shadowM my-5 px-25 py-4 flex">
			<image src="../../static/images/posters-img2.png" class="img1"></image>
			<view class="flex5 h120 flex-1 pl-2">
				<view class="font-w">{{mainData.title?mainData.title:''}}</view>
				<view class="font-24 color6 flex-1">{{mainData.execution?mainData.execution:''}}</view>
				<view class="flex color9 font-22">
					<view>查看详情</view>
					<image src="../../static/images/details-icon7.png" class="B-icon ml-1"></image>
				</view>
			</view>
		</view>
		
		
	
		
		<view style="height: 120rpx;"></view>
		
	
		
		<!-- 预览展示 -->
		<view class="p-2 p-fX bottom-0 shadowT bg-white flex1 tt">
			<view class="btn80-M" @click="Router.back(1)">返回编辑</view>
			<button class="btn80 Mgb colorf" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">发布</button>
		</view>
		
		
		
		<!-- 发布弹窗 -->
		<view class="bg-mask line-h flex0" v-show="public_show">
			<view class="mask radius10 px-3 py-4 flex4 p-r pp">
				<view class="font-30 font-w pt-4">温馨提示</view>
				<view class="font-24 color6 py-5 line-h-md">
					发布成功，赶快呼朋唤友，<br />
					邀请小伙伴一起来参与吧！
				</view>
				<view class="btn80 Mgb colorf w-100 mt-5" @click="Show(2)">邀请朋友参与</view>
				
				<view class="p-3 p-a right-0 top-0"  @click="Show(1)">
					<image src="../../static/images/toapplyfor-icon4.png" class="wh24"></image>
				</view>
			</view>
		</view>
		
		<!-- 发布成功分享弹窗 -->
		<view class="bg-mask flex0" v-show="share_show">
			<view class="mm">
				<view class="bg-white radius10 maskBox">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode="widthFix"></image>
					<view class="px-3 pt-4">
						<view class="txt font-24">“{{submitData.explain?submitData.explain:''}}”</view>
						<view class="font-26 color9 text-r">--{{submitData.name?submitData.name:''}}</view>
					</view>
					<view class="flex p-3">
						<image src="../../static/images/my-img.png" class="wh80 radus-5"></image>
						<view class="font-22 line-h flex-1 pl-2">
							<view class="color9 pb-2">请和我一起支持</view>
							<view>【{{mainData.title?mainData.title:''}}】</view>
						</view>
						<image src="../../static/images/certificate-img.png" class="wh110"></image>
					</view>
				</view>
				<view class="flex1 py-5">
					<view class="btn80 bg-white colorM">保存成图片</view>
					<button class="btn80 Mgb colorf" open-type="share">邀请朋友参与</button>
				</view>
				<view class="mt-2 p-3 flex0" @click="Show(2)">
					<image src="../../static/images/xx.png" class="wh34"></image>
				</view>
			</view>
		</view>
		
		<view class="pc-container" style="position: fixed;left:0;top: 0;z-index: 9999;" v-show="canvasShow">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas"  :style="{width: width+'px',height:height+'px'}"></canvas>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				public_show:false,
				jk_show:false,
				share_show:true,
				submitData:{
					
				},
				
				mainData:{},
				Utils:this.$Utils,
				canvasShow: true,
				qrUrl: '',
				rpx: 0,
				width:0,
				height:0,
			}
		},
		onLoad(options) {
			const self = this;
			self.submitData = uni.getStorageSync('togetherSubmit')
			self.id = options.id
			self.$Utils.loadAll(['getMainData'], self);
			
			//获取屏幕宽度，获取自适应单位
			wx.getSystemInfo({
				success: function(res) {
					self.rpx = res.windowWidth / 375;
					self.width = 330* self.rpx 
					self.height = 350*self.rpx
					console.log('self.width',self.width)
				},
			})
			
		},
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: '跟'+self.submitData.name+'一起做好事，我加入了，你来吗',
					path: '/pages/goTogether/goTogether?id=' + self.teamId, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData && self.mainData.mainImg && self.mainData.mainImg[0] && self.mainData.mainImg[0].url ?
						self.mainData.mainImg[0].url : '',
				}
			} else {
				return {
					title: '跟'+self.submitData.name+'一起做好事，我加入了，你来吗',
					path: '/pages/goTogether/goTogether?id=' + self.teamId, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData && self.mainData.mainImg && self.mainData.mainImg[0] && self.mainData.mainImg[0].url ?
						self.mainData.mainImg[0].url : '',
				}
			}
		},
		methods: {
			
			writeTextOnCanvas(ctx_2d, lineheight, bytelength, text, startleft, starttop) {
				function getTrueLength(str) { //获取字符串的真实长度（字节长度）
					var len = str.length,
						truelen = 0;
					for (var x = 0; x < len; x++) {
						if (str.charCodeAt(x) > 128) {
							truelen += 2;
						} else {
							truelen += 1;
						}
					}
					return truelen;
				}
			
				function cutString(str, leng) { //按字节长度截取字符串，返回substr截取位置
					var len = str.length,
						tlen = len,
						nlen = 0;
					for (var x = 0; x < len; x++) {
						if (str.charCodeAt(x) > 128) {
							if (nlen + 2 < leng) {
								nlen += 2;
							} else {
								tlen = x;
								break;
							}
						} else {
							if (nlen + 1 < leng) {
								nlen += 1;
							} else {
								tlen = x;
								break;
							}
						}
					}
					return tlen;
				}
				for (var i = 1; getTrueLength(text) > 0; i++) {
					var tl = cutString(text, bytelength);
					ctx_2d.fillStyle = '#222';
					ctx_2d.font = 'bold 12px Arial';
					ctx_2d.fillText(text.substr(0, tl).replace(/^\s+|\s+$/, ""), startleft, (i - 1) * lineheight + starttop);
					text = text.substr(tl);
				}
			},
			
			canvasImage() {
				const self = this;
				self.isCom = false;
				uni.showLoading({
					title: '正在生成图片',
					mask: true
				})
				var myCanvas = uni.createCanvasContext('mycanvas', this);
				myCanvas.fillStyle = "#FFFFFF";
				myCanvas.fillRect(0, 0, 330*self.rpx, 350*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				var mainImg = this.mainData.mainImg[0].url;
				uni.getImageInfo({
					src: mainImg, //网络路径
					success: function(res) {
						
						self.isCom1 = true;
						myCanvas.drawImage(res.path, 0, 0, 330*self.rpx, 185*self.rpx);
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					}
				});
				
				this.writeTextOnCanvas(myCanvas, 20, 40*self.rpx, '“'+self.submitData.explain+'”', 15*self.rpx, 210*self.rpx)
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				myCanvas.font = '13px Arial'
				myCanvas.fillStyle = '#999';
				//myCanvas.setTextAlign('right')
				myCanvas.fillText('- '+self.submitData.name, 290*self.rpx,255*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				
				var headImg = uni.getStorageSync('user_info').headImgUrl;
				uni.getImageInfo({
					src: headImg, //网络路径
					success: function(res) {
						var r = 20*self.rpx;
						var x = 15*self.rpx;
						var y = 290*self.rpx;
						var d =2 * r;
						var cx = x + r;
						var cy = y + r;
						myCanvas.arc(cx, cy, r, 0, 2 * Math.PI);
						myCanvas.clip();
						myCanvas.drawImage(res.path, x, y, d, d);
						self.isCom2 = true;
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					}
				});
				
				return
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.font = 'bold 18px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText('MAY BEE', 175 *self.rpx, 30*self.rpx, 350 *self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				var bannerImg = this.mainData.bannerImg[0].url;
				uni.getImageInfo({
					src: bannerImg, //网络路径
					success: function(res) {
						console.log('bannerImg', res)
						self.isCom1 = true;
						myCanvas.drawImage(res.path, 20*self.rpx, 70*self.rpx, 307*self.rpx, 307*self.rpx);
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					}
				});
				myCanvas.setTextAlign('left')
				this.writeTextOnCanvas(myCanvas, 20, 35*self.rpx, self.mainData.title, 20*self.rpx, 425*self.rpx)
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				myCanvas.font = 'bold 16px Arial'
				myCanvas.fillStyle = '#EF2732';
				myCanvas.fillText('￥' + self.mainData.price, 20*self.rpx, 470*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				var qr = self.qrUrl;
				uni.getImageInfo({
					src: qr,
					complete(res)  {
						console.log('res',res)
						self.isCom2 = true;
						myCanvas.drawImage(res.path, 275*self.rpx,
							400*self.rpx, 60*self.rpx, 60*self.rpx);
							
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					}
					
				});
				console.log('self.isCom2',self.isCom2)
				console.log('self.isCom1',self.isCom1)
				var interval = setInterval(function() {
					self.num++
					if (self.isCom2&&self.isCom1) {
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
			
			getQrCode(id) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: id,
					page: 'pages/togetherOrder/togetherOrder',
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
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				const callback = (user, res) => {
					console.log(res)
					self.teamAdd();
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			teamAdd(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					project_id:self.id,
					price:self.submitData.price,
					purpose:self.submitData.purpose,
					name:self.submitData.name,
					explain:self.submitData.explain
				};
				postData.refreshToken = true;
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code==100000) {
						self.teamId = res.info.id;
						self.Show(1)
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.teamAdd(postData, callback);
			},
			
			
			Show(type){
				const self = this;
				if(type==0){
					self.jk_show = !self.jk_show;
				}else if(type==1){
					self.public_show = !self.public_show;
				}else{
					self.share_show = !self.share_show;
					self.public_show = false;
				}
			},
			
			getMainData(isNew) {
				var self = this;
				var postData = {};
				postData.searchItem = {
					id:self.id
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						self.getQrCode()
						self.canvasImage()
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.projectGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.img{height: 440rpx;}
.con{margin-top: -50rpx;}
.img1{width: 160rpx;height: 120rpx;border-radius: 10rpx;}
.h120{height: 120rpx;}
.tt .btn80,.btn80-M{width: 340rpx;}
.btn80-M{border: 1px solid #D5D5D5;}

.mm,.pp{width: 660rpx;}
.maskBox{width: 660rpx;max-height: 80%;overflow-y: auto;}
.txt{min-height: 80rpx;}
.mm .btn80{width: 310rpx;margin: 0;}

.jk{height: 420rpx;}
.jkItem{width: 160rpx;line-height: 80rpx;text-align: center;margin: 0 22rpx 20rpx 0;}
.jkItem:nth-child(4n){margin-right: 0;}
input{flex: 1;font-size: 30rpx;}
</style>
