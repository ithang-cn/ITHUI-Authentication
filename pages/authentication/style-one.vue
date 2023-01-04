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
		<view class="form">
			<view class="form-item">
				<label>姓名</label>
				<input v-model="userInfo.name" maxlength="20" placeholder="请输入姓名" autocomplete="off" class="input" />
			</view>
			<view class="form-item">
				<label>身份证号码</label>
				<input v-model="userInfo.code" maxlength="18" type="idcard" placeholder="请输入身份证号码" autocomplete="off"
					class="input" />
			</view>
			<view class="form-item">
				<label>银行预留手机号</label>
				<input v-model="userInfo.phone" maxlength="11" type="number" placeholder="请输入银行预留手机号" autocomplete="off"
					class="input" />
			</view>
			<view class="form-item icon-right">
				<label>开户银行</label>
				<input @click="showBankList = true" :value="userInfo.bank" type="text" disabled placeholder="请选择开户银行"
					autocomplete="off" class="input" />
			</view>
			<view class="form-item">
				<label>结算卡号</label>
				<input v-model="userInfo.banknum" maxlength="19" type="number" placeholder="请输入结算储蓄卡卡号"
					autocomplete="off" class="input" />
			</view>
			<u-picker title="请选择开户银行" closeOnClickOverlay :show="showBankList" :columns="bankList"
				@cancel="showBankList = false" @confirm="checkBank">
			</u-picker>
		</view>
		<view class="upload">
			<view class="upload-item">
				<u-upload @afterRead="afterRead($event)" :fileList="uploadA" @delete="deletePic" name="uploadA" multiple
					:maxCount="1" width="160" height="120">
					<image src="@/static/style-one/sfzzm.jpg" mode="widthFix" style="width: 160px;height: 120px;">
					</image>
				</u-upload>
				<text class="title">身份证正面</text>
			</view>
			<view class="upload-item">
				<u-upload @afterRead="afterRead($event)" :fileList="uploadB" @delete="deletePic" name="uploadB" multiple
					:maxCount="1" width="160" height="120">
					<image src="@/static/style-one/sfzfm.jpg" mode="widthFix" style="width: 160px;height: 120px;">
					</image>
				</u-upload>
				<text class="title">身份证反面</text>
			</view>
			<view class="upload-item">
				<u-upload @afterRead="afterRead($event)" :fileList="uploadC" @delete="deletePic" name="uploadC" multiple
					:maxCount="1" width="160" height="120">
					<image src="@/static/style-one/yhk.jpg" mode="widthFix" style="width: 160px;height: 120px;">
					</image>
				</u-upload>
				<text class="title">银行卡正面</text>
			</view>
			<view class="upload-item">
				<u-upload @afterRead="afterRead($event)" :fileList="uploadD" @delete="deletePic" name="uploadD" multiple
					:maxCount="1" width="160" height="120">
					<image src="@/static/style-one/scsfz.jpg" mode="widthFix" style="width: 160px;height: 120px;">
					</image>
				</u-upload>
				<text class="title">手持身份证银行卡正面</text>
			</view>
		</view>
		<view class="submit">
			<u-button @click="submit" type="warning" text="提交实名"></u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					name: '',
					code: '',
					phone: '',
					bank: '',
					banknum: ''
				},
				uploadA: [],
				uploadB: [],
				uploadC: [],
				uploadD: [],
				showBankList: false,
				bankList: [
					['工商银行', '建设银行', '农业银行', '中国银行']
				]
			}
		},
		onLoad() {},
		methods: {
			checkBank(e) {
				this.userInfo.bank = e.value
				this.showBankList = false
			},
			// 选择图片
			afterRead(event) {
				this[event.name] = event.file
			},
			// 删除图片
			deletePic(event) {
				this[event.name] = []
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
						title: '请输入身份证号码',
						icon: 'none',
						duration: 2000
					});
				} else if (!uni.$u.test.idCard(this.userInfo.code)) {
					uni.showToast({
						title: '身份证号码格式不正确',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.userInfo.phone) {
					uni.showToast({
						title: '请输入银行预留手机号',
						icon: 'none',
						duration: 2000
					});
				} else if (!uni.$u.test.mobile(this.userInfo.phone)) {
					uni.showToast({
						title: '手机号格式不正确',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.userInfo.bank) {
					uni.showToast({
						title: '请选择开户银行',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.userInfo.banknum) {
					uni.showToast({
						title: '请输入储蓄卡卡号',
						icon: 'none',
						duration: 2000
					});
				} else if (!uni.$u.test.rangeLength(this.userInfo.banknum, [16, 19])) {
					uni.showToast({
						title: '储蓄卡卡号格式不正确',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.uploadA) {
					uni.showToast({
						title: '请上传身份证正面',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.uploadB) {
					uni.showToast({
						title: '请上传身份证反面',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.uploadC) {
					uni.showToast({
						title: '请上传银行卡正面',
						icon: 'none',
						duration: 2000
					});
				} else if (!this.uploadD) {
					uni.showToast({
						title: '请上传手持身份证银行卡正面',
						icon: 'none',
						duration: 2000
					});
				} else {
					const uploads = [this.uploadA, this.uploadB, this.uploadC, this.uploadD]
					const form = {
						...this.userInfo,
						url: []
					}
					uploads.forEach(item => {
						this.uploadFilePromise(item).then(res => {
							form.url.push(res)
						})
					})
					if (form.url.length !== 4) {
						uni.showToast({
							title: '图片上传失败，请稍后重试',
							icon: 'none',
							duration: 2000
						});
					} else {
						console.log(form)
					}
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
		background-color: #fff;
	}
</style>

<style lang="scss" scoped>
	.ithui {
		&-content {
			.form {
				padding: 0 20rpx;
				padding-bottom: 48rpx;

				&-item {
					height: 100rpx;
					display: flex;
					justify-content: space-between;
					align-items: center;
					font-size: 34rpx;
					border-bottom: 2rpx solid #eeeeee;
					color: #333333;
					padding: 0 20rpx;
					width: 100%;

					label {
						flex-shrink: 0;
						padding-right: 20rpx;
					}

					.input {
						height: 40rpx;
						text-align: right;
						padding-right: 50rpx;
						width: 100%;
					}
				}

				.icon-right:after {
					content: "";
					background: url(@/static/arrow-right.png) center right no-repeat;
					position: relative;
					top: 0;
					right: 40rpx;
					height: 100%;
					display: inline-block;
					background-size: 15rpx 30rpx;
					width: 30rpx;
				}

			}

			.upload {
				display: flex;
				justify-content: space-between;
				flex-wrap: wrap;
				padding: 0 25rpx;

				&-item {
					width: 310rpx;
					padding: 20rpx;
					text-align: center;

					.title {
						color: #999999;
						font-size: 28rpx;
						line-height: 28rpx;
					}
				}
			}

			.submit {
				margin-top: 40rpx;
			}
		}
	}
</style>
