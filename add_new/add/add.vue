<template>
	<view class="container">
		<form @submit="suces" @reset="failer">
			<view class="id">
				<text>序号：</text>
				<input type="text" value="" />
			</view>
			<view class="img">
				<text>图片:</text>
				<view class="uni-uploader__input-box">
					
					<view class="uni-uploader__input" @tap="chooseImage"><image class="uni-uploader__img" :src="image" :data-src="image" @tap="previewImage"></image></view>
					
				</view>
				<!-- <view class="imgs">预览图</view> -->
			</view>
			<view class="name">
				<text>名称：</text>
				<input type="text" value="" />
			</view>
			<view class="brand">
				<text>型号：</text>
				<input type="text" value="" />
			</view>
			<view class="pin">
				<text>品牌：</text>
			<input type="text" value="" />
			</view>
			<view class="bz">
				<text>备注：</text>
				<textarea value="" placeholder="" />
			</view>
			<view class="btns">
				<button form-type="submit" class="confim uni-badge-primary" size="mini">确认修改</button>
				<button form-type="reset" class="back uni-badge-danger" size="mini">取消返回</button>
			</view>
			
		</form>
	</view>
</template>

<script>
	import permision from "@/common/permission.js"
	var sourceType = [
		['camera'],
		['album'],
		['camera', 'album']
	]
	var sizeType = [
		['compressed'],
		['original'],
		['compressed', 'original']
	]
	export default {
		data() {
			return {
				title: 'choose/previewImage',
				imageList: [],
				sourceTypeIndex: 2,
				sourceType: [],
				sizeTypeIndex: 2,
				sizeType: [],
				countIndex: 8,
				count: [1, 2, 3, 4, 5, 6, 7, 8, 9],
				image:""
			}
		},
		onUnload() {
			this.imageList = []
		},
		methods: {
			suces:function(){
				uni.showToast({
					icon:'success',
					title:'提交成功'
				})
			},
			failer:function(){
				uni.navigateBack({
					delta:3
				})
			},
			chooseImage: async function() {
				
				uni.chooseImage({
					sourceType: sourceType[this.sourceTypeIndex],
					sizeType: sizeType[this.sizeTypeIndex],
					count: this.imageList.length + this.count[this.countIndex] > 9 ? 9 - this.imageList.length : this.count[this.countIndex],
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					},
					fail: (err) => {
						// #ifdef APP-PLUS
						if (err['code'] && err.code !== 0 && this.sourceTypeIndex === 2) {
							this.checkPermission(err.code);
						}
						// #endif
						// #ifdef MP
						uni.getSetting({
							success: (res) => {
								let authStatus = false;
								switch (this.sourceTypeIndex) {
									case 0:
										authStatus = res.authSetting['scope.camera'];
										break;
									case 1:
										authStatus = res.authSetting['scope.album'];
										break;
									case 2:
										authStatus = res.authSetting['scope.album'] && res.authSetting['scope.camera'];
										break;
									default:
										break;
								}
								if (!authStatus) {
									uni.showModal({
										title: '授权失败',
										content: 'Hello uni-app需要从您的相机或相册获取图片，请在设置界面打开相关权限',
										success: (res) => {
											if (res.confirm) {
												uni.openSetting()
											}
										}
									})
								}
							}
						})
						// #endif
					}
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
		}
	}
</script>

<style>
	.container{
		background: #F8F8F8;
	}
	form{
		width:100%;height:360px;
		padding:20px;
	}
	form view{
		width:100%;height:60rpx;
		display: flex;
		justify-content: flex-start;
		flex-direction: row;
		align-items: center;
		padding:20px;
	}
	form view text{
		display: inline-block;
		width:20%;height:60rpx;
		font-size: 28rpx;
		color:#666;
	}
	form view input{
		width:60%;height:72rpx;
		border:2rpx solid #D9D9D9;
		background: #fff;
	}
	form view textarea{
		width:60%;height:80px;
		border:2rpx solid #D9D9D9;
		background: #fff;
	}
	form .btns{
		width:100%;height:60rpx;
		display: flex;
		justify-content: space-around;
		flex-direction: row;
		align-items: center;
	}
	form .img .uni-uploader__input-box{
		width:50%;height:150rpx;
		margin:20rpx;padding:0rpx;
		margin-left:0rpx;
	}
	form .confim,form  .back{
		width:36%;height:60rpx;
	}
</style>
