<template>
	<div @longpress="long">
		<!-- 时间显示 -->
		<view v-if="showTime" 
		class="flex align-center justify-center pb-4 pt-2">
			<text class="font-sm text-light-muted">{{showTime}}</text>
		</view>
		<!-- 撤回消息 -->
		<view v-if="item.isremove" ref="isremove"
		class="flex align-center justify-center pb-4 pt-1 chat-animate">
			<text class="font-sm text-light-muted">你撤回了一条信息</text>
		</view>
		<!-- 气泡 -->
		<view v-else class="flex align-start position-relative mb-3"
		:class="!isself ? 'justify-start' : 'justify-end'">
			<!-- 好友 -->
			<template v-if="!isself">
				<free-avater size="70" :src="item.avatar"></free-avater>
				
				<text v-if="hasLabelClass" class="iconfont text-white font-md position-absolute chat-left-icon">&#xe609;</text>
			</template>
			
			<div class="p-2 rounded" :class="labelClass" style="max-width:500rpx;">
				<!-- 文字 -->
				<text v-if="item.type === 'text'" class="font-md">{{item.data}}</text>
				<!-- 表情包 -->
				<image v-else-if="item.type === 'emoticon'" :src="item.data"
				lazy-load mode="widthFix" style="width: 300rpx;height: 300rpx;"></image>
				
			</div>
			<!-- 本人 -->
			<template v-if="isself">
				<text v-if="hasLabelClass" class="iconfont text-chat-item font-md position-absolute chat-right-icon">&#xe640;</text>
				<free-avater size="70" :src="item.avatar"></free-avater>
			</template>
		</view>
		
	</div>
</template>

<script>
	import freeAvater from "@/components/free-ui/free-avater.vue"
	import $T from "@/common/free-lib/time.js"
	export default {
		components: {
			freeAvater
		},
		props: {
			item: Object,
			index:Number,
			// 上一条消息的时间戳
			pretime:[Number,String]
		},
		computed: {
			// 是否是本人
			isself() {
				// 获取本人id（假设拿到了）
				let id = 1
				return this.item.user_id === id 
			},
			// 显示的时间
			showTime(){
				return $T.getChatTime(this.item.create_time,this.pretime)
			},
			// 是否需要气泡样式
			hasLabelClass(){
				return this.item.type === 'text' || this.item.type === 'audio'
			},
			// 气泡的样式
			labelClass(){
				let label = this.hasLabelClass ? 'bg-chat-item mr-3' : 'mr-3'
				return this.isself ? label : 'bg-white ml-3'
			}
		},
		mounted() {
			// 监听是否撤回消息
			// #ifdef APP-PLUS-NVUE
			this.$watch('item.isremove',(newVal,oldVal)=>{
				if (newVal) {
					const animation = weex.requireModule('animation')
					this.$nextTick(()=>{
						animation.transition(this.$refs.isremove, {
						    styles: {
								opacity:1
						    },
						    duration: 100, //ms
						    timingFunction: 'ease',
						    }, function () {
						       console.log('动画执行结束');
						    })
					})
				}
			})
			// #endif
		},
		methods:{
			// 长按事件
			long(e){
				let x = 0
				let y = 0
				// #ifdef APP-PLUS-NVUE
				if (Array.isArray(e.changedTouches) && e.changedTouches.length > 0) {
					x = e.changedTouches[0].screenX
					y = e.changedTouches[0].screenY
				}
				// #endif
				// #ifdef MP
				x = e.detail.x
				y = e.detail.y
				// #endif
				this.$emit('long',{
					x,
					y,
					index:this.index
				})
			}
		}
	}
</script>

<style scoped>
	.chat-left-icon{
		left: 80rpx;top: 20rpx;
	}
	.chat-right-icon{
		right: 80rpx;
		top: 20rpx;
	}
	.chat-animate{
		/* #ifndef APP-PLUS-NVUE */
		opacity: 0;
		/* #endif */
	}
</style>
