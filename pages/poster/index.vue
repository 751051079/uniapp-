<template>
	<view class="page">
		<view class="flex flex-align flex-column">
			<text class="m-top-20 m-bottom-20">海报(圆头像，标题超过两行，三行省略)</text>
			<canvas canvas-id="canvasId" :style="{'width':`${widthSize}px`,'height':`${widthSize*1.7}px`}"></canvas>
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
				ctx:'',
				title:'关注关注关注关注关注关注关注关注关注关注关注',
				futitle:'扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注扫描二维码关注'
			}
		},
		onLoad() {
			that=this
			uni.getSystemInfo({
				success: function(e) {
					//获取生成图片像素比
					that.pixelRatio=e.pixelRatio
					//获取画布的大小
					that.widthSize=e.windowWidth*0.9
				}
			})
		},
		mounted() {
			that.drawPosters()
		},
		methods: {
			//保存相册到本地
			saveImage(){
				let wid=that.widthSize
				uni.canvasToTempFilePath({
				  x: 0,
				  y: 0,
				  width: wid,
				  height: wid*1.7,
				  destWidth: wid*that.pixelRatio,
				  destHeight: wid*1.7*that.pixelRatio,
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
				that.ctx = uni.createCanvasContext('canvasId', this);
				let wid=that.widthSize
				that.ctx.drawImage('https://ymxsx-imgs.oss-accelerate.aliyuncs.com/xinsuoxiang/images/2021/11/11/16366154867888443.png',0,0,wid,wid*1.7)
				// that.ctx.beginPath()
				that.ctx.drawImage(`../../static/collectionCode/code.png`,wid/4.44,wid/1.7,wid/1.93,wid/1.93)   
				let titleLength=''
				let arr=[]
				if(that.title.length>18){
					that.ctx.setFontSize(15)
					titleLength=that.ctx.measureText(`${that.title.slice(0,18)}`).width
					that.ctx.fillText(`${that.title.slice(0,18)}`,(wid-titleLength)/2, wid/3.2)
					if(that.title.length>36){
						that.ctx.setFontSize(15)
						titleLength=that.ctx.measureText(`${that.title.slice(18,36)}...`).width
						that.ctx.fillText(`${that.title.slice(18,36)}...`,(wid-titleLength)/2, wid/2.7)
					}
					else{
						that.ctx.setFontSize(15)
						titleLength=that.ctx.measureText(`${that.title.slice(18)}`).width
						that.ctx.fillText(`${that.title.slice(18)}`,(wid-titleLength)/2, wid/2.7)
					}
				}else{
					that.ctx.setFontSize(15)
					titleLength=that.ctx.measureText(`${that.title}`).width
					that.ctx.fillText(`${that.title}`,(wid-titleLength)/2, wid/3.2)
				}
				if(that.futitle.length>=42){
					that.ctx.setFontSize(13)
					titleLength=that.ctx.measureText(`${that.futitle.slice(0,21)}`).width
					that.ctx.fillText(`${that.futitle.slice(0,21)}`,(wid-titleLength)/2, wid/2.3)
					that.ctx.setFontSize(13)
					titleLength=that.ctx.measureText(`${that.futitle.slice(21,42)}`).width
					that.ctx.fillText(`${that.futitle.slice(21,42)}`,(wid-titleLength)/2, wid/2)
					that.ctx.setFontSize(13)
					if(that.futitle.length>63){
						titleLength=that.ctx.measureText(`${that.futitle.slice(42,60)}...`).width
						that.ctx.fillText(`${that.futitle.slice(42,60)}...`,(wid-titleLength)/2, wid/1.78)
					}else{
						titleLength=that.ctx.measureText(`${that.futitle.slice(42)}`).width
						that.ctx.fillText(`${that.futitle.slice(42)}`,(wid-titleLength)/2, wid/1.7)
					}
					
				}else if(that.futitle.length>=21 && that.futitle.length<42){
					that.ctx.setFontSize(13)
					titleLength=that.ctx.measureText(`${that.futitle.slice(0,21)}`).width
					that.ctx.fillText(`${that.futitle.slice(0,21)}`,(wid-titleLength)/2, wid/2.3)
					that.ctx.setFontSize(13)
					titleLength=that.ctx.measureText(`${that.futitle.slice(21)}`).width
					that.ctx.fillText(`${that.futitle.slice(21)}`,(wid-titleLength)/2, wid/2)
				}
				else{
					that.ctx.setFontSize(13)
					titleLength=that.ctx.measureText(`${that.futitle}`).width
					that.ctx.fillText(`${that.futitle}`,(wid-titleLength)/2, wid/2.3)
				}
				
				that.ctx.arc(wid/2,wid/13.34+(wid/5.25/2),wid/5.25/2,0,2 * Math.PI)
				that.ctx.clip();
				that.ctx.drawImage('../../static/logo.jpg',(wid-wid/5.25)/2,wid/13.34,wid/5.25,wid/5.25)
				that.ctx.restore()
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
