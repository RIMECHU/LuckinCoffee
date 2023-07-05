<template>
	<view class="order">
		<view class="nav">
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" @clickLeft="goBack" :fixed="true"
				title="我的订单"></uni-nav-bar>
		</view>
		<view class="blue"></view>

		<view class="status">
			<view class="all" :class="index==0?'active':''" @click="changeIndex(0)">全部</view>
			<view class="going" :class="index==1?'active':''" @click="changeIndex(1)">进行中</view>
			<view class="finish" :class="index==2?'active':''" @click="changeIndex(2)">已完成</view>
		</view>
		
		<view class="order-msg">
			<view class="all"  v-for="(item,index) in orderProduct" :key="item.id">
				<view class="order-title">{{item.oid}}</view>
				<view class="product-item">
					<view class="item-left">
						<image :src="item.smallImg" mode=""></image>
					</view>
					<view class="item-right">
						<view class="name-rules">
							<text class="name">{{item.name}}</text>
							<text class="rules">{{item.rule}}</text>
						</view>
						<view class="enname">{{item.enname}}</view>
						<view class="price-num">
							<text class="price">￥{{item.price}}</text>
							<text class="num">x{{item.count}}</text>
						</view>
					</view>
				</view>
			</view>
			<view class="time-price">
				<text class="left-ball"></text>
				<text class="right-ball"></text>
		
				<view class="now-time">{{this.time}}</view>
				<view class="count-allPrice">
					<text class="count">共计{{allCount}}件</text>
					<text class="allPrice">合计￥{{allPrice}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				index: 0,
				token:'',
				orderProduct: [],
				// time: "",
				allCount: 0,
				allPrice: 0,
			}
		},
		
		
		onShow() {
			this.token = uni.getStorageSync('token')
			this.getOrder();
			
			let date = new Date()
			
			let year = date.getFullYear();
			let month = date.getMonth() + 1;
			let day = date.getDate();
			let hours = date.getHours();
			let min = date.getMinutes();
			let sec = date.getSeconds();
			
			this.time = `${year}-${month}-${day} ${hours}:${min}:${sec}`
			console.log('this.time', this.time)
		},
		
		methods: {
			goBack() {
				uni.navigateBack({
					delta: 1 //返回上一级
				})
			},
			
			changeIndex(value) {
				this.index = value
				this.getOrder();
			},

			getOrder() {
				if (!this.token) {
					uni.navigateTo({
						url: "/pages/login/login"
					})
					return
				}
				
				uni.request({
					url: "http://www.kangliuyong.com:10002/findOrder",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						tokenString: this.token,
						status: this.index
					},
					success: (res) => {
						console.log("res", res)
						
						this.orderProduct = res.data.result
						
						this.orderProduct.map(item => {
							this.allCount += item.count
							this.allPrice += item.count * item.price
						})
					},
					fail: (err) => {
						console.log("err", err)
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.order {
		.nav {
			/deep/ .uni-icons {
				color: #0c34ba !important;
			}

			/deep/ .uni-navbar-btn-text span {
				color: #0c34ba !important;
			}
		}

		.blue {
			height: 200rpx;
			background: #0c34ba;
		}

		.status {
			display: flex;
			height: 100rpx;
			background: white;
			margin: 0 20rpx;
			margin-top: -30rpx;
			border-radius: 30rpx 30rpx 0 0;
			justify-content: space-around;
			align-items: center;
			font-size: 28rpx;
			color: #646566;

			view {
				padding: 0 10rpx;
				height: 90%;
				line-height: 100rpx;
				text-align: center;
			}

			.active {
				border-bottom: 5rpx solid #0c34ba;
				color: #0c34ba;
			}
		}
		
		.order-msg {
			margin: 20rpx;
			padding: 20rpx;
			padding-bottom: 50rpx;
			// height: 600rpx;
			background: white;
			border-radius: 30rpx 30rpx 0 0;
		
			font-size: 28rpx;
			color: #717273;
			
			.all{
				.order-title{
					margin: 10rpx 0;
				}
				
				.product-item {
					margin-top: 20rpx;
					display: flex;
						
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
						
						.rules{
							color: #b7bdb9;
						}
						
						.enname {
							overflow: hidden;
							white-space: nowrap;
							text-overflow: ellipsis;
							font-size: 22rpx;
							color: #b7bdb9;
						}
						
						.price-num {
							margin-top: 30rpx;
							padding-right: 60rpx;
							display: flex;
							justify-content: space-between;
						
							.price {
								font-weight: 600;
								color: #0c34ba;
							}
						}
					}
				}
			}
			
		
			.time-price {
				position: relative;
				margin-top: 20rpx;
				height: 100rpx;
				border-top: 1rpx dashed #e8e8e8;
		
				.right-ball,
				.left-ball {
					position: absolute;
					height: 40rpx;
					width: 40rpx;
					background: #f6f6f6;
					border-radius: 50%;
					top: -20rpx;
				}
		
				.left-ball {
					left: -40rpx;
				}
		
				.right-ball {
					right: -40rpx;
				}
		
				.now-time {
					margin-top: 40rpx;
					margin-bottom: 20rpx;
					font-size: 24rpx;
				}
		
				.count-allPrice {
					display: flex;
					justify-content: space-between;
		
					.allPrice {
						font-weight: 600;
						color: #0c34ba;
					}
				}
			}
		}
	}
</style>