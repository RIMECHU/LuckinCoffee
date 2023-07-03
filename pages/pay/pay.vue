<template>
	<view class="pay">
		<view class="nav">
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" :fixed="true" title="订单结算"></uni-nav-bar>
		</view>
		
		<view class="choose-address">
			<view class="choose-btn">
				<text>选择地址</text>
				<uni-icons type="forward" size="18" color="#646566"></uni-icons>
			</view>
			<view class="user-msg">
				<text class="username">RIM</text>
				<text class="phone">17688272891</text>
				<text class="default">默认</text>
			</view>
			<view class="user-address">广东省佛山市南海区西樵镇</view>
		</view>
		
		<view class="order-msg">
			<view class="order-title">订单信息</view>
			<view class="product-item">
				<view class="item-left">
					<image src="../../static/home.png" mode=""></image>
				</view>
				<view class="item-right">
					<view class="name-rules">
						<text class="name">标准美式</text>
						<text class="rules">冷/单份糖</text>
					</view>
					<view class="enname">Americano</view>
					<view class="price-num">
						<text class="price">￥22.00</text>
						<text class="num">x1</text>
					</view>
				</view>
			</view>
			<view class="time-price">
				<text class="left-ball"></text>
				<text class="right-ball"></text>
				
				<view class="now-time">2023-07-03 16:50:00</view>
				<view class="count-allPrice">
					<text class="count">共计6件</text>
					<text class="allPrice">合计￥162</text>
				</view>
			</view>
		</view>
		
		<view class="btn-buy">
			<view class="btn">立即结算</view>
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
	.pay {

		.nav {
			/deep/ .uni-icons {
				color: #0c34ba !important;
			}

			/deep/ .uni-navbar-btn-text span {
				color: #0c34ba !important;
			}
		}
		
		.choose-address{
			margin: 20rpx;
			// height: 200rpx;
			background: white;
			border-radius: 20rpx 20rpx 0 0;
			padding: 20rpx;
			font-size: 28rpx;
			color: #717273;
			
			.choose-btn{
				display: flex;
				align-items: center;
			}
			
			.user-msg{
				display: flex;
				align-items: center;
				margin: 20rpx 0;
				.username{
					color: #0c34ba;
					font-weight: 600;
				}
				
				.phone{
					margin: 0 40rpx;
				}
				
				.default{
					height: 50rpx;
					background: #0c34ba;
					color: white;
					padding: 0 15rpx;
					line-height: 46rpx;
					border-radius: 30rpx;
					font-size: 24rpx;
				}
			}
			
			.user-address{
				
			}
		}
	
		.order-msg{
			margin: 20rpx;
			padding: 20rpx;
			padding-bottom: 50rpx;
			// height: 600rpx;
			background: white;
			border-radius: 30rpx 30rpx 0 0;
			
			font-size: 28rpx;
			color: #717273;
			
			.product-item{
				margin-top: 20rpx;
				display: flex;
				
				.item-left{
					width: 150rpx;
					height: 150rpx;
					background: white;
					margin-right: 20rpx;
					
					image{
						width: 100%;
						height: 100%;
					}
				}
				
				.item-right{
					flex: 1;
					height: 150rpx;
					background: white;
					
					.name{
						margin-right: 30rpx;
						color: #65676b;
					}
					
					.enname{
						font-size: 26rpx;
					}
					
					.price-num{
						margin-top: 30rpx;
						padding-right: 60rpx;
						display: flex;
						justify-content: space-between;
						
						.price{
							font-weight: 600;
							color: #0c34ba;
						}
					}
				}
			}
		
			.time-price{
				position: relative;
				margin-top: 20rpx;
				height: 100rpx;
				border-top: 1rpx dashed #e8e8e8;
				
				.right-ball,.left-ball{
					position: absolute;
					height: 40rpx;
					width: 40rpx;
					background: #f6f6f6;
					border-radius: 50%;
					top: -20rpx;	
				}
				
				.left-ball{
					left: -40rpx;
				}
				
				.right-ball{
					right: -40rpx;
				}
				
				.now-time{
					margin-top: 40rpx;
					margin-bottom: 20rpx;
					font-size: 24rpx;
				}
				
				.count-allPrice{
					display: flex;
					justify-content: space-between;
					
					.allPrice{
						font-weight: 600;
						color: #0c34ba;
					}
				}
			}
		
			
		}
	
		.btn-buy{
			position: fixed;
			bottom: 0;
			height: 120rpx;
			width: 100%;
			background: #f6f6f6;
			
			.btn{
				margin: 0 20rpx;
				background: #0c34ba;
				font-size: 28rpx;
				color: white;
				height: 90rpx;
				text-align: center;
				line-height: 90rpx;
				border-radius: 50rpx;
			}
		}
	}
</style>