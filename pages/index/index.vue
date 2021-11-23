<template>
	<view class="page">
		<view class="flex flex-column">
			<text class="m-top-20 m-bottom-20 text-decoration" @click="goLink('/pages/collectionCode/index')">收款码</text>
			<text class="m-top-20 m-bottom-20 text-decoration" @click="goLink('/pages/poster/index')">海报(圆头像，标题超过两行，三行省略)</text>
			
		</view>
	</view>
</template>

<script>
	let that
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
			that=this
			
		},
		mounted() {
			
		},
		methods: {
			goLink(url){
				uni.navigateTo({
					url:url
				})
			}
		}
	}
</script>

<style>
	.page {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.flex{
		flex-flow: row;
		display: flex;
	}
	.flex-align {
		align-items: center;
	}
	.flex-column {
		flex-direction: column;
	}
	.m-top-20{
		margin-top: 20rpx;
	}
	.m-bottom-20{
		margin-bottom: 20rpx;
	}
	.text-decoration{
		text-decoration: underline;
	}
</style>
