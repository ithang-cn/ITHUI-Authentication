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
		<view class="progress">
			<view class="list">
				<view class="list-item" :class="{active: step >= stepIndex + 1}" v-for="(item,stepIndex) in stepList"
					:key="stepIndex">
					<view class="icon"></view>
					<view class="text">{{item}}</view>
				</view>
			</view>
		</view>
		<view class="container" v-if="step == 2">
			<u-modal :show="noteShow" title="注意事项" confirmText="我已知晓" @confirm="noteShow = false"
				@close="noteShow = false">
				<view class="slot-content">
					<rich-text :nodes="content"></rich-text>
				</view>
			</u-modal>
			<view class="box">
				<view class="form">
					<view class="form-input">
						<view class="name">真实姓名</view>
						<input v-model="userInfo.name" type="text" autocomplete="off" placeholder="请输入真实姓名"
							maxlength="20">
					</view>
					<view class="form-input">
						<view class="name">身份证号</view>
						<input v-model="userInfo.code" type="idcard" autocomplete="off" placeholder="请输入身份证号"
							maxlength="18">
					</view>
					<view class="form-title">
						<text>上传身份证照片</text>
						<view class="tip" @click="noteShow = true">
							<u-icon name="error-circle-fill" color="#eca419" size="18"></u-icon>
							注意事项
						</view>
					</view>
					<view class="upload-box">
						<view class="upload-item">
							<u-upload @afterRead="afterRead" :fileList="uploadA" @delete="deletePic" name="uploadA"
								multiple :maxCount="1" width="130" height="80">
								<image src="@/static/style-three/idcard-front.png"
									style="width: 130px;height: 80px;padding: 10px;">
								</image>
							</u-upload>
							<view class="intro">拍摄正面</view>
						</view>
						<view class="upload-item">
							<u-upload @afterRead="afterRead" :fileList="uploadB" @delete="deletePic" name="uploadB"
								multiple :maxCount="1" width="130" height="80">
								<image src="@/static/style-three/idcard-behind.png"
									style="width: 130px;height: 80px;padding: 10px;">
								</image>
							</u-upload>
							<view class="intro">拍摄反面</view>
						</view>
					</view>
					<view class="form-tip">
						<text style="font-size: 32rpx;color: #333;">温馨提示：</text>拍摄时确保身份证 <text style="color: #ff5050;">
							边框完整，字体清晰，亮度均匀</text>
					</view>
					<view class="form-help"><text>无法通过认证？</text> <text style="color: #3c7bfb;"
							@click="help">点击获得帮助</text></view>
					<view class="form-submit"><u-button @click="submit" type="primary" text="确认提交" size="large"
							shape="circle"></u-button>
					</view>
				</view>
			</view>
			<view class="copyright">
				本系统由 ITHANG 提供技术支持
			</view>
		</view>
		<view class="step-other" v-else>
			<!-- 自定义其他步骤的内容 -->
			<u-empty icon="https://static.ithang.cn/ITHUI/empty-page.png" text="当前步骤还没有内容..." width="250" textSize="24">
			</u-empty>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					name: '',
					code: ''
				},
				uploadA: [],
				uploadB: [],
				stepList: ['账号注册', '实名认证', '设置密码', '绑定银行卡', '认证成功'],
				step: 2, // 当前步骤序号
				noteShow: false,
				content: `<div>1、大陆公民持本人有效的<span style="color: #ff5050;">二代身份证</span></div><div>2、拍摄时确保身份证<span style="color: #ff5050;">边框完整，字体清晰，亮度均匀</span></div>`
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
			help() {
				uni.showToast({
					title: '正在前往帮助页面...',
					icon: 'none',
					duration: 2000
				});
			},
			async submit() {
				if (!this.userInfo.name) {
					uni.showToast({
						title: '请输入姓名',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.userInfo.code) {
					uni.showToast({
						title: '请输入身份证号',
						icon: 'none',
						duration: 2000
					});
				} else if (!uni.$u.test.idCard(this.userInfo.code)) {
					uni.showToast({
						title: '身份证号格式有误',
						icon: 'none',
						duration: 2000
					});
				} else if (this.uploadA.length === 0) {
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
						url: 'http://192.168.1.100/upload', // 仅为示例，非真实的接口地址
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
		background-color: #3c7bfb;
		color: #222;
	}
</style>

<style lang="scss" scoped>
	.ithui {
		&-content {
			.progress {
				padding: 10rpx 6rpx;
				font-size: 28rpx;

				.list {
					display: flex;
					justify-content: space-between;
					align-items: center;
					list-style: none;

					.list-item {
						width: 100%;
						position: relative;
						text-align: center;
						color: #77a3fc;

						.icon {
							display: inline-block;
							width: 14rpx;
							height: 14rpx;
							background: #ffffff;
							border-radius: 100%;
							position: relative;
							margin-top: 40rpx;
							margin-bottom: 20rpx;

						}

						&:not(:last-child):after {
							content: "";
							position: absolute;
							width: 124rpx;
							height: 6rpx;
							background: #77a3fc;
							border-radius: 2rpx;
							top: 43rpx;
							left: 87rpx;
							z-index: 97;
						}
					}

					.active {
						color: #f8f8f8;

						&:after {
							background: #f8f8f8 !important;
						}

						.icon:after {
							content: "";
							position: absolute;
							top: 50%;
							left: 50%;
							transform: translate(-50%, -50%);
							display: block;
							width: 40rpx;
							height: 40rpx;
							background: url(../../static/style-three/icon-point-active.png) center center no-repeat;
							background-size: 100% 100%;
							z-index: 98;
						}
					}
				}
			}

			.container {
				height: auto;
				overflow: hidden;
				padding: 10rpx 30rpx;

				.box {
					height: 1000rpx;
					background: url(../../static/style-three/box-bg.png) right center no-repeat;
					background-size: 100% 100%;

					.form {
						padding: 40rpx 24rpx;

						&-input {
							height: 28rpx;
							display: flex;
							justify-content: space-between;
							align-items: center;
							background: #f7f7f7;
							font-size: 32rpx;
							padding: 30rpx 10rpx;
							margin-bottom: 2rpx;

							.name {
								color: #222;
								flex-shrink: 0;
								padding-left: 30rpx;
								padding-right: 55rpx;
							}

							input {
								text-indent: 6rpx;
								width: 100%;
								background: transparent;
								color: #333;
							}
						}

						&-title {
							font-size: 32rpx;
							color: #000;
							display: flex;
							align-items: center;
							padding: 85rpx 0 50rpx 0;

							.tip {
								color: #eca419;
								font-size: 24rpx;
								display: flex;
								align-items: center;
								margin-left: 30rpx;
							}
						}

						.upload {
							&-box {
								display: flex;
								align-items: center;
								justify-content: space-between;
								flex-wrap: wrap;
							}

							&-item {
								background: #f4f8fe;
								border-radius: 20rpx;

								.intro {
									background: #5581ff;
									border-radius: 0 0 20rpx 20rpx;
									text-align: center;
									color: #fff;
									font-size: 32rpx;
									padding: 12rpx 0;
								}
							}
						}

						&-tip {
							color: #999;
							font-size: 28rpx;
							margin: 60rpx 0 20rpx 0;
						}

						&-help {
							font-size: 28rpx;
							color: #9d9d9d;
							text-align: center;
						}

						&-submit {
							text-align: center;
							padding: 30rpx 40rpx;
						}
					}
				}

				.copyright {
					text-align: center;
					font-size: 28rpx;
					color: #97b9fe;
					padding: 35rpx 0;
				}
			}
		}
	}
</style>