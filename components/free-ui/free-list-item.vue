<template>
	<view class="bg-white flex align-stretch" hover-class="bg-light"
	@click="$emit('click')">
		<view class="flex align-center justify-center py-2 px-3">
			<image :src="cover" v-if="cover"
			mode="widthFix" style="width: 75rpx;height: 75rpx;"></image>
		</view>
		<view class="flex-1 border-bottom flex align-center">
			<text class="font-md text-dark">{{title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			// 封面
			cover: {
				type: String,
				default: ""
			},
			// 标题
			title:{
				type:String,
				default:""
			}
		},
	}
</script>

<style>
</style>
