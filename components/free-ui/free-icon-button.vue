<template>
	<view class="flex align-center justify-center" 
	hover-class="bg-hover-light" @click="$emit('click')"
	style="height: 90rpx;width: 90rpx;">
		<rich-text class="iconfont font-md"><slot></slot></rich-text>
	</view>
</template>

<script>
</script>

<style>
</style>
