<template>
	<view class="flex align-center justify-center" 
	hover-class="bg-hover-light" @click="$emit('click')"
	style="height: 90rpx;width: 90rpx;">
		<text class="iconfont font-md"><slot></slot></text>
	</view>
</template>

<script>
</script>

<style>
</style>
