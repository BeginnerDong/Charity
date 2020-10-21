<template>
	<view>
		
		<view class="p-3">
			<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:0,
				mainData:{}
			}
		},
		onLoad(options) {
			const self = this;
			if(options.type){
				if(options.type==0){
					self.changeTit('捐款须知')
				}
				if(options.type==1){
					self.changeTit('用户须知')
				}
			}
			self.type = options.type;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			changeTit(title){
				uni.setNavigationBarTitle({
				    title: title
				});
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:self.type==0?'捐款须知':'用户须知'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>

<style>

</style>
