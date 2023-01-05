/**
* ITHUI®
* ITHUI-Authentication 适用于上传身份认证、实名认证、完善认证资料，兼容小程序、APP、H5。
* Copyright (c) 2023 ITHANG All rights reserved.
* Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
* 复制使用请保留本段注释，感谢支持开源！
*
* 作者主页
* https://ithang.cn/
*
* 开源地址
* https://github.com/ithang-cn/ITHUI-Authentication
* https://gitee.com/ithang-cn/ITHUI-Authentication
*
*/

<template>
	<view class="ithui-content">
		<view class="upload">
			<h2>请拍摄并上传你的有效身份证照片</h2>
			<view class="upload-box">
				<view class="upload-item">
					<u-upload @afterRead="afterRead" :fileList="uploadA" @delete="deletePic" name="uploadA" multiple
						:maxCount="1" width="145" height="115">
						<image src="@/static/style-two/sfzzm.png" style="width: 130px;height: 90px;padding: 15px;">
						</image>
					</u-upload>
					<view class="intro">上传身份证正面</view>
				</view>
				<view class="upload-item">
					<u-upload @afterRead="afterRead" :fileList="uploadB" @delete="deletePic" name="uploadB" multiple
						:maxCount="1" width="145" height="115">
						<image src="@/static/style-two/sfzfm.png" style="width: 130px;height: 90px;padding: 15px;">
						</image>
					</u-upload>
					<view class="intro">上传身份证正面</view>
				</view>
			</view>
		</view>
		<view class="mt-20 expect">
			<h2>拍摄身份证要求：</h2>
			<view class="expect-text">大陆公民持有的本人有效二代身份证；</view>
			<view class="expect-text">
				拍摄时确保身份证 <text>边框完整，字体清晰，亮度均匀；</text>
			</view>
			<view class="error">
				<div class="error-item">
					<image src="@/static/style-two/yesok.png" />
					<text>标准</text>
				</div>
				<div class="error-item">
					<image src="@/static/style-two/error_01.png" />
					<text>边框缺失</text>
				</div>
				<div class="error-item">
					<image src="@/static/style-two/error_02.png" />
					<text>边框模糊</text>
				</div>
				<div class="error-item">
					<image src="@/static/style-two/error_03.png" />
					<text>闪光强烈</text>
				</div>
			</view>
		</view>
		<view class="confirm">
			<h2 class="mt-20 b-line">请确认你的身份信息：</h2>
			<view class="confirm-item">
				<view class="key">姓名</view>
				<text class="val">{{userInfo.name}}</text>
			</view>
			<view class="mt-20 confirm-item">
				<view class="key">身份证号</view>
				<text class="val">{{userInfo.code}}</text>
			</view>
			<view class="mt-20 confirm-item">
				<view class="key">有效期</view>
				<text class="val">{{userInfo.end}}</text>
			</view>
		</view>
		<view class="submit">
			<u-button @click="submit" shape="circle" type="primary" text="身份认证"></u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					name: '张三',
					code: '330228********4399',
					end: '2029-12-31'
				},
				uploadA: [],
				uploadB: []
			}
		},
		onLoad() {},
		methods: {
			// 选择图片
			afterRead(event) {
				this[event.name] = event.file
			},
			// 删除图片
			deletePic(event) {
				this[event.name] = []
			},
			async submit() {
				if (this.uploadA.length === 0) {
					uni.showToast({
						title: '请上传身份证正面',
						icon: 'none',
						duration: 2000
					});
				} else if (this.uploadB.length === 0) {
					uni.showToast({
						title: '请上传身份证反面',
						icon: 'none',
						duration: 2000
					});
				} else {
					const uploads = [this.uploadA, this.uploadB]
					const form = {
						...this.userInfo,
						url: []
					}
					uploads.forEach(item => {
						this.uploadFilePromise(item[0]).then(res => {
							form.url.push(res)
						}).catch(err => {
							uni.showToast({
								title: '图片上传失败，请稍后重试',
								icon: 'none',
								duration: 2000
							});
							return
						})
					})
					console.log(form)
				}
			},
			uploadFilePromise(event) {
				return new Promise((resolve, reject) => {
					let a = uni.uploadFile({
						url: 'http://192.168.2.21/upload', // 仅为示例，非真实的接口地址
						filePath: event,
						name: 'file',
						formData: {},
						success: (res) => {
							setTimeout(() => {
								resolve(res.data.data)
							}, 1000)
						},
						fail: () => {
							reject()
						}
					});
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #f5f7fc;
	}
</style>

<style lang="scss" scoped>
	.ithui {
		&-content {
			.mt-20 {
				margin-top: 20rpx;
			}

			.upload {
				background: #fff;

				&-box {
					display: flex;
					justify-content: center;
					align-items: center;
					flex-wrap: wrap;
					padding: 0 20rpx 30rpx 20rpx;
				}

				h2 {
					color: #999;
					font-weight: normal;
					text-align: left;

					font-size: 32rpx;
					padding: 20rpx 0 30rpx 40rpx;
				}

				&-item {
					background: #f4f8fe;
					border-radius: 20rpx;
					margin: 0 16rpx;

					.intro {
						background: #5581ff;
						border-radius: 0 0 20rpx 20rpx;
						text-align: center;
						color: #fff;
						font-size: 28rpx;
						padding: 16rpx 0;
					}
				}
			}

			.expect {
				color: #333;
				font-size: 26rpx;
				padding: 40rpx 30rpx 10rpx 30rpx;
				background: #fff;

				h2 {
					color: #999999;
					font-weight: normal;
					font-size: 32rpx;
					margin-bottom: 20rpx;
				}

				&-text {
					text {
						font-style: normal;
						color: #ff5050;
					}
				}

				.error {
					display: flex;
					justify-content: space-between;
					align-items: center;

					&-item {
						text-align: center;
						padding: 20rpx 0;
						flex: 1;
						margin: 20rpx 5rpx;

						image {
							width: 160rpx;
							height: 120rpx;
							margin: 0 auto;
						}

						text {
							font-size: 24rpx;
							color: #919191;
						}
					}
				}
			}

			.confirm {
				h2 {
					color: #999;
					font-weight: normal;
					font-size: 32rpx;
					padding: 30rpx;
					background-color: #fff;
				}

				&-item {
					display: -webkit-box;
					display: -webkit-flex;
					display: flex;
					-webkit-box-align: center;
					-webkit-align-items: center;
					align-items: center;
					padding: 30rpx;
					position: relative;
					background-color: #fff;

					.key {
						-webkit-box-flex: 1;
						-webkit-flex: 1;
						flex: 1;
						min-width: 0;
						font-size: 32rpx;
						color: #333;
						font-weight: bold;
					}

					.val {}
				}

				.b-line {
					position: relative;

					&:after {
						content: '';
						position: absolute;
						z-index: 2;
						bottom: 0;
						left: 0;
						width: 100%;
						height: 2rpx;
						border-bottom: 2rpx solid #e2e2e2;
						transform: scaleY(0.5);
						transform-origin: 0 100%;
						-webkit-transform: scaleY(0.5);
						-webkit-transform-origin: 0 100%;
					}
				}
			}

			.submit {
				margin-top: 20rpx;
				padding: 20px;
			}
		}
	}
</style>
