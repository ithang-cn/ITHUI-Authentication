/**
* ITHUI®
* ITHUI-Authentication 适用于上传身份认证、完善认证资料、实名认证、完善认证资料，兼容小程序、APP、H5。
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
		<view style="margin: 60rpx 40rpx;">
			<u-button @click="navTo('/pages/authentication/style-one')" type="success"
				text="样式一 完善认证资料（身份证+银行卡）"></u-button>
		</view>
		<view style="margin: 60rpx 40rpx;">
			<u-button @click="navTo('/pages/authentication/style-two')" type="error" text="样式二 身份证实名认证（身份证）"></u-button>
		</view>
		<view style="margin: 60rpx 40rpx;">
			<u-button @click="navTo('/pages/authentication/style-three')" type="primary"
				text="样式三 上传身份证实名认证（身份证）"></u-button>
		</view>
		<view style="margin: 60rpx 40rpx;">
			<u-button @click="navTo('/pages/authentication/style-four')" type="warning" text="样式四 实名认证"></u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {},
		methods: {
			navTo(url) {
				uni.navigateTo({
					url
				});
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
		&-content {}
	}
</style>