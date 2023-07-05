<template>
	<view class="my">
		<view class="img-box">
			<image src="../../static/isj.jpg" mode="widthFix"></image>
		</view>

		<view class="user-msg">
			<view class="user-title">
				<view class="user-left">
					<view class="img-box">
						<image class="img" src="../../static/rim.jpg" mode="widthFix"></image>
					</view>
				</view>
				<view class="user-right">
					<view class="user-right-title">Rim</view>
					<view class="user-right-desc">迷之双目的少女!</view>
				</view>
			</view>

			<view class="choose-all">
				<view class="choose-category" @click="toUserMsg()">
					<view>个人资料</view>
					<uni-icons type="forward" size="18" color="#646566"></uni-icons>
				</view>
				<view class="choose-category">
					<view>我的订单</view>
					<uni-icons type="forward" size="18" color="#646566"></uni-icons>
				</view>
				<view class="choose-category">
					<view>我的收藏</view>
					<uni-icons type="forward" size="18" color="#646566"></uni-icons>
				</view>
				<view class="choose-category">
					<view>地址管理</view>
					<uni-icons type="forward" size="18" color="#646566"></uni-icons>
				</view>
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
			toUserMsg(){
				uni.navigateTo({
					url:"/pages/UserMsg/UserMsg"
				})
			}
		}
	}
</script>

<style lang="scss">
	.my {
		background: #f7f7f7;

		.img-box {
			height: 400rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.user-msg {
			margin-top: -80px !important;
			position: relative;
			// top: 0;
			border-radius: 20rpx 20rpx 0 0;
			padding: 20rpx 30rpx;
			// padding-bottom: 50rpx;
			margin: 0 20rpx;
			overflow: hidden;
			//透明边框
			background-image: linear-gradient(rgba(255, 255, 255, 0.3), #ffffff, #ffffff);

			.user-title {
				margin-bottom: 75rpx;
				display: flex;

				.user-left {
					flex: 3;

					.img-box {
						width: 150rpx;
						height: 150rpx;
						border-radius: 50%;
						overflow: hidden;

						.img {
							width: 100%;
						}
					}
				}
			}

			.user-right {
				flex: 7;

				.user-right-title {
					color: #0c34ba;
					font-size: 32rpx;
					font-weight: 600;
				}

				.user-right-desc {
					margin-top: 10rpx;
					color: #999999;
					font-size: 28rpx;
				}
			}
			
			.choose-all {
				margin-top: 100rpx;
				.choose-category {
					color: #999999;
					font-size: 28rpx;
					border-bottom: 1rpx solid #e8e8e8;
					padding: 30rpx 0;
					display: flex;
					justify-content: space-between;
					align-items: center;
				}
				//单独消除最后一条线
				.choose-category:last-child{
					border-bottom:none;
				}
			}
		}
	}
</style>