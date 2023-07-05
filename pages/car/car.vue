<template>
	<view class="car">
		<view class="nav">
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" right-text="编辑" @clickLeft="goBack"
				:fixed="true" title="购物袋" size="16"></uni-nav-bar>
		</view>
		<view>
			<image src="../../static/shopbag_bg.png" mode="widthFix"></image>
		</view>
		<view class="order">
			<view>
				<u-checkbox-group>
					<u-checkbox model="" shape="circle" size="21"></u-checkbox>
				</u-checkbox-group>
			</view>
			<view class="product-item">
				<view class="item-left">
					<image src="../../static/home.png" mode=""></image>
				</view>
				<view class="item-right">
					<view class="name-rules">
						<text class="name">标准美式</text>
						<text class="rules">冷/单份糖</text>
					</view>
					<view class="enname">Americno</view>
					<view class="price-num">
						<text class="price">￥22.00</text>
						<view class="btn-box">
							<image class="ruse" src="../../static/mun.png" mode="" @click="minus()"></image>
							<view class="num">1</view>
							<image class="add" src="../../static/add.png" mode="" @click="add"></image>
						</view>
					</view>
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

		}
	}
</script>

<style lang="scss">
	.car {
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

		image {
			width: 100%;
		}

		.order {
			z-index: 100;
			position: relative;
			align-items: center;
			display: flex;
			margin: 20rpx;
			padding: 20rpx;
			padding-bottom: 50rpx;
			// height: 600rpx;
			background: white;
			border-radius: 30rpx 30rpx 0 0;
			

			.product-item {
				display: flex;
				// margin-top: 30rpx;

				.item-left {
					width: 150rpx;
					height: 150rpx;
					background: white;
					margin-right: 20rpx;

					image {
						width: 100%;
						height: 100%;
					}
				}

				.item-right {
					flex: 1;
					height: 150rpx;
					background: white;

					.name {
						margin-right: 30rpx;
						color: #65676b;
					}

					.rules {
						font-size: 20rpx;
						color: #acacac;
					}

					.enname {
						overflow: hidden;
						white-space: nowrap;
						text-overflow: ellipsis;
						font-size: 20rpx;
						color: #acacac;
					}

					.price-num {
						margin-top: 30rpx;
						padding-right: 60rpx;
						display: flex;
						justify-content: space-between;
						align-items: center;

						.price {
							font-weight: 600;
							color: #0c34ba;
							font-size: 26rpx;
						}
						
						.btn-box {
							display: flex;
							align-items: center;
							margin-left: 80px;
							
						
							.num {
								margin: 0 20rpx;
							}
						
							image {
								width: 40rpx;
								height: 40rpx;
							}
						}
					}
				}
			}
		}
	}
</style>