<template>
	<view>
		
		<view class="p-3">
			协议内容
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:0
			}
		},
		onLoad(options) {
			const self = this;
			if(options.type){
				if(options.type==0){
					self.changeTit('捐款须知')
				}
				if(options.type==1){
					self.changeTit('用户协议')
				}
			}
			self.type = options.type;
		},
		methods: {
			
			changeTit(title){
				uni.setNavigationBarTitle({
				    title: title
				});
			}
			
		}
	}
</script>

<style>

</style>
