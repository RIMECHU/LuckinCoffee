<template>
	<view class="UserMsg">
		<view class="nav">
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" @clickLeft="goBack" :fixed="true" title="个人资料"
				size="16"></uni-nav-bar>
		</view>
		
		<view class="background"></view>
		
		<view class="user-msg">
			<view class="user-head">
				<view class="user-left">
					<view class="user-left-title">头像</view>
				</view>
				<view class="user-right">
					<view class="img-box">
						<image class="img" src="../../static/rim.jpg" mode="widthFix"></image>
					</view>
				</view>
			</view>
			
				<view class="user-desc">
					<view>用户id</view>
					<view class="user-desc-detail">coffee17688272891</view>
				</view>
				<view class="user-desc">
					<view>昵称</view>
					<view class="user-desc-detail">Rim</view>
				</view>
				<view class="user-desc">
					<view>简介</view>
					<view class="user-desc-detail">迷之双目的少女!</view>
				</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			goBack() {
				uni.navigateBack({
					delta: 1 //返回上一级
				})
			},
		}
	}
</script>

<style lang="scss">
	.UserMsg {
		.nav {
			/deep/ .uni-icons {
				color: #0c34ba !important;
			}

			/deep/ .uni-navbar-btn-text span {
				color: #0c34ba !important;
				font-size: 28rpx;
			}

			/deep/ .uni-nav-bar-text {
				font-size: 16px;
			}
		}

		.background {
			width: 100%;
			height: 200rpx;
			background: #0c34ba;
		}

		.user-msg {
			height: 400rpx;
			background: white;
			margin-top: -20px !important;
			position: relative;
			// top: 0;
			border-radius: 20rpx 20rpx 0 0;
			padding: 25rpx 30rpx;
			// padding-bottom: 50rpx;
			margin: 0 20rpx;
			overflow: hidden;
			font-size: 28rpx;
			color: #323233;

			.user-head {
				padding: 10rpx 0;
				display: flex;
				align-items: center;
				justify-content: space-between;
				border-bottom: 1rpx solid #f5f6f7;
				
				.user-right{
					// position: absolute;
					.img-box {
						width: 70rpx;
						height: 70rpx;
						border-radius: 50%;
						overflow: hidden;
					
						.img {
							width: 100%;
						}
					}
				}
			}
			
			.user-desc{
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 30rpx 0;
				border-bottom: 1rpx solid #f5f6f7;
				text-align: center;
				
				.user-desc-detail{
					color: #969799;
				}
			}
			.user-desc:last-child{
				border-bottom:none;
			}
		}
	}
</style>