<template>
	<view class="flex align-center justify-center" 
	hover-class="bg-hover-light" @click="$emit('click')"
	style="height: 90rpx;width: 90rpx;">
		<rick-text class="iconfont font-md"><slot></slot></rick-text>
	</view>
</template>

<script>
</script>

<style>
</style>
