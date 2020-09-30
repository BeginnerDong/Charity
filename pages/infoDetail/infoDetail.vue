<template>
	<view class="px-25">
		
		<view class="py-3 bB-f5">
			<view class="font-40">{{mainData.title?mainData.title:''}}</view>
			<view class="font-24 color9 pt-2">{{mainData.create_time?mainData.create_time:''}}</view>
		</view>
		
		<view class="py-3 font-26">
			<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				searchItem:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.searchItem.id = options.id
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						self.mainData.create_time = self.mainData.create_time.substr(0,10)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>

<style>

</style>
