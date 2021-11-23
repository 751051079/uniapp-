<template>
	<view class="page">
		<view class="flex flex-align flex-column">
			<text class="m-top-20 m-bottom-20">收款码</text>
			<canvas canvas-id="canvasId" :style="{'width':`${widthSize}px`,'height':`${widthSize}px`}"></canvas>
		</view>
		<view class="flex flex-center btn" @click="saveImage">
			<text style="color: #FFFFFF; font-size: 30rpx;">保存到相册</text>
		</view>
	</view>
</template>

<script>
	let that
	export default {
		data() {
			return {
				pixelRatio:1,
				widthSize:0,
				ctx:''
			}
		},
		onLoad() {
			that=this
			uni.getSystemInfo({
				success: function(e) {
					//获取生成图片像素比
					that.pixelRatio=e.pixelRatio
					//获取画布的大小
					that.widthSize=e.windowWidth*0.8
				}
			})
		},
		mounted() {
			that.drawPosters()
		},
		methods: {
			//保存到相册
			saveImage(){
				let wid=that.widthSize
				uni.canvasToTempFilePath({
				  x: 0,
				  y: 0,
				  width: wid,
				  height: wid,
				  destWidth: wid*that.pixelRatio,
				  destHeight: wid*that.pixelRatio, 
				  canvasId: 'canvasId',
				  success: function(res) {
				    // 在H5平台下，tempFilePath 为 base64
				    console.log(res.tempFilePath)
					uni.saveImageToPhotosAlbum({
						filePath: res.tempFilePath,
						success: function() {
							uni.showToast({
								title:"保存到相册成功",
								icon:"none"
							})
						},
						fail(e) {
							uni.showToast({
								title:"保存到相册失败",
								icon:"none"
							})
						}
					});
				  } 
				})
			},
			//绘制海报的方法
			drawPosters(){
				//初始化画布
				that.ctx = uni.createCanvasContext('canvasId', this);
				//然后需要确定比例，设计图一般都是750的屏幕，这里是375px
				let wid=that.widthSize
				//背景图
				that.ctx.drawImage('../../static/collectionCode/code-bg.png',0,0,wid,wid)
				//二维码图
				that.ctx.drawImage(`../../static/collectionCode/code.png`,wid*0.24,wid*0.23,wid*0.51,wid*0.52)
				//四个角的边框图片
				that.ctx.drawImage(`../../static/collectionCode/code-border.png`,wid*0.21,wid*0.2,wid*0.58,wid*0.7)
				//绘制完成
				that.ctx.draw()
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
	.flex-center {
		align-items: center;
		justify-content: center;
	}
	.btn{
		width: 710rpx;
		height: 70rpx;
		background-color: #007AFF;
		border-radius: 12rpx;
		margin-top: 100rpx;
	}
</style>
