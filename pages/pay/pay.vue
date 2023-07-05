<template>
	<view class="pay">
		<view class="nav">
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" @clickLeft="goBack" :fixed="true"
				title="订单结算"></uni-nav-bar>
		</view>

		<view class="choose-address">
			<view class="choose-btn" @click="open()">
				<text>选择地址</text>
				<uni-icons type="forward" size="18" color="#646566"></uni-icons>
			</view>
			<view class="user-msg">
				<text class="username">{{name}}</text>
				<text class="phone">{{phone}}</text>
				<text class="default" v-if="isDefault">默认</text>
			</view>
			<view class="user-address">{{joinAddress}}</view>
		</view>

		<view class="order-msg">
			<view class="order-title">订单信息</view>
			<view class="product-item" v-for="(item,index) in orderProduct" :key="item.sid">
				<view class="item-left">
					<image :src="item.small_img" mode=""></image>
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

		<view class="btn-buy">
			<view class="btn" @click="pay">立即结算</view>
		</view>

		<uni-popup ref="popup" type="bottom" background-color="#fff">
			<view class="content">
				<view class="nav-bottom">
					<text>选择地址</text>
					<uni-icons type="closeempty" size="18" color="#646566"></uni-icons>
				</view>

				<view class="ad-fram">
					<view class="address" @click="changeAddress(index)" v-for="(item,index) in addressArr"
						:key="item.aid">
						<u-checkbox-group>
							<u-checkbox @change="changeAddress(index)" :checked="item.hook" shape="circle"
								size="16"></u-checkbox>
						</u-checkbox-group>

						<view class="address-msg">
							<view class="user">
								<text>{{item.name}}</text>
								<text class="phone">{{item.tel}}</text>
								<text class="default" v-if="item.isDefault==1">默认</text>
							</view>
							<view class="msg">{{item.province}}{{item.city}}{{item.county}}{{item.addressDetail}}</view>
						</view>
					</view>
				</view>


				<view class="add-address">新增地址</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				token: '',
				sids: '',
				orderProduct: [],
				time: "",
				allCount: 0,
				allPrice: 0,

				addressArr: [],
				joinAddress: "",
				name: "",
				phone: "",
				isDefault: false
			}
		},

		onLoad(options) {
			console.log("options", options)
			this.sids = options.sid;
		},

		onShow() {
			//获取token
			this.token = uni.getStorageSync('token')
			this.getPayProduct();
			// console.log("this.token",this.token)
			this.getAddress();

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
			pay() {
				if (this.name.length == 0) {
					return
					uni.showToast({
						title: "请选择地址",
						icon: "error"
					})
				}

				uni.request({
					url: "http://www.kangliuyong.com:10002/pay",
					method: "POST",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						tokenString: this.token,
						sids: this.sids,
						phone: this.phone,
						address: this.joinAddress,
						receiver: this.name
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset=utf-8' // 默认值
					},
					success: (res) => {
						console.log('res', res)

						if (res.data.code == 60000) {
							uni.redirectTo({
								url: "/pages/order/order"
							})
						} else {
							uni.showToast({
								title: "支付失败",
								icon: "error"
							})
						}
					},
					fail: (err) => {
						console.log('err', err)
					},
				})
			},

			changeAddress(index) {
				this.addressArr = this.addressArr.map((item, count) => {

					item.hook = false

					if (index == count) {
						item.hook = true,

							this.joinAddress = item.province + item.city + item.county + item.addressDetail
						this.name = item.name
						this.phone = item.tel
						this.isDefault = true
					}

					return item
				})
			},

			getAddress() {
				if (!this.token) {
					uni.navigateTo({
						url: "/pages/login/login"
					})
					return
				}

				uni.request({
					url: "http://www.kangliuyong.com:10002/findAddress",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						tokenString: this.token,
					},
					success: (res) => {
						console.log("res", res)

						this.addressArr = res.data.result.map(item => {

							item.hook = false

							if (item.isDefault == 1) {
								item.hook = true,

									this.joinAddress = item.province + item.city + item.county + item
									.addressDetail
								this.name = item.name
								this.phone = item.tel
								this.isDefault = true
							}

							return item
						})

					},
					fail: (err) => {
						console.log("err", err)
					}
				})
			},

			open() {
				// 通过组件定义的ref调用uni-popup方法 ,如果传入参数 ，type 属性将失效 ，仅支持 ['top','left','bottom','right','center']
				this.$refs.popup.open()
			},

			goBack() {
				uni.navigateBack({
					delta: 1 //返回上一级
				})
			},

			getPayProduct() {
				if (!this.token) {
					uni.navigateTo({
						url: "/pages/login/login"
					})
					return
				}

				uni.request({
					url: "http://www.kangliuyong.com:10002/commitShopcart",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						tokenString: this.token,
						sids: this.sids
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
	.pay {

		.nav {
			/deep/ .uni-icons {
				color: #0c34ba !important;
			}

			/deep/ .uni-navbar-btn-text span {
				color: #0c34ba !important;
			}
		}

		.choose-address {
			margin: 20rpx;
			// height: 200rpx;
			background: white;
			border-radius: 20rpx 20rpx 0 0;
			padding: 20rpx;
			font-size: 28rpx;
			color: #717273;

			.choose-btn {
				display: flex;
				align-items: center;
			}

			.user-msg {
				display: flex;
				align-items: center;
				margin: 20rpx 0;

				.username {
					color: #0c34ba;
					font-weight: 600;
				}

				.phone {
					margin: 0 40rpx;
				}

				.default {
					height: 40rpx;
					background: #0c34ba;
					color: white;
					padding: 0 15rpx;
					line-height: 40rpx;
					border-radius: 30rpx;
					font-size: 24rpx;
				}
			}

			.user-address {}
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

					.enname {
						overflow: hidden;
						white-space: nowrap;
						text-overflow: ellipsis;
						font-size: 26rpx;
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

		.btn-buy {
			position: fixed;
			bottom: 0;
			height: 120rpx;
			width: 100%;
			background: #f6f6f6;

			.btn {
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

		.content {
			padding: 10rpx 20rpx 10rpx 20rpx;

			.nav-bottom {
				display: flex;
				align-items: center;
				justify-content: space-between;
				margin: 10rpx 0;
			}

			.ad-fram {
				height: 400rpx;
				//滚动条
				overflow: scroll;
			}

			.add-address {
				margin: 10rpx 30rpx;
				height: 80rpx;
				background: #0c34ba;
				color: white;
				text-align: center;
				line-height: 80rpx;
				border-radius: 50rpx;
				margin-bottom: 20rpx;
			}

			.address {
				padding: 30rpx 0;
				margin-bottom: 40rpx;
				display: flex;
				border-bottom: 1rpx solid #e8e8e8;
				color: #323233;

				.address-msg {
					margin-left: 20rpx;
				}

				.msg {
					margin-top: 20rpx;
					font-size: 26rpx;
				}

				.phone {
					margin: 0 20rpx;
				}

				.default {
					display: inline-block;
					height: 46rpx;
					background: #0c34ba;
					color: white;
					padding: 0 10rpx;
					line-height: 46rpx;
					border-radius: 30rpx;
					font-size: 24rpx;
				}
			}
		}
	}
</style>