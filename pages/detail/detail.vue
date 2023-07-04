<template>
	<view class="detail">
		<view class="nav-top">
			<!-- <view class="go-back" @click="goBack"> -->
				<!-- <image src="../../static/home_active.png" mode=""></image>
				<text>返回</text>
			</view>
			<view class="product-title">商品详情</view> -->
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" @clickLeft="goBack"  :fixed="true" title="商品详情"></uni-nav-bar>
		</view>

		<view class="detail-img">
			<image :src="detailData.large_img" mode="widthFix"></image>
		</view>

		<view class="center-content">
			<view class="title-price">
				<view class="title">{{detailData.name}}</view>
				<view class="price">￥{{detailData.price}}</view>
			</view>

			<view class="en-name">{{detailData.enname}}</view>
			<view class="rules-box">
				<view class="type-box" v-for="(item,index) in typeData">
					<view class="type">{{item.type}}</view>
					<view class="type-value">
						<text v-for="(value,count) in item.typeValue" @click="changeIndex(index,count)"
							:class="{'active':item.index==count}" :key="count">{{value}}</text>
					</view>
				</view>
			</view>
			<view class="choose-count">
				<view class="choose-text">选择数量</view>
				<view class="btn-box">
					<image class="ruse" src="../../static/mun.png" mode="" @click="minus()"></image>
					<view class="num">{{num}}</view>
					<image class="add" src="../../static/add.png" mode="" @click="add"></image>
				</view>
			</view>
			<view class="product-instr">
				<view class="goods-des">商品描述</view>
				<view class="goods-item" v-for="(item,index) in detailData.desc">{{index+1}}、{{item}}</view>
			</view>
		</view>

		<view class="footer">
			<view class="footer-left">
				<view class="car">
					<image src="../../static/shopbag_active.png" mode=""></image>
					<view>购物袋</view>
				</view>
				<view class="collect">
					<image src="../../static/menu_active.png" mode=""></image>
					<view>已收藏</view>
				</view>
			</view>
			<view class="footer-right">
				<view class="add-car" @click="addCar">加入购物袋</view>
				<view class="go-buy" @click="gobuy">立即购买</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

				detailData: {},
				typeData: [],
				num: 1,

				token: "",

				pid: ""
			}
		},

		onLoad(options) {
			console.log("options", options)
			this.getDetail(options.pid)
			this.pid = options.pid
		},

		onShow() {
			//获取token
			this.token = uni.getStorageSync('token')
		},

		methods: {
			addCar(buy) {
				let rule = ""
				let ruleArr = this.typeData.map(item => {
					return item.typeValue[item.index]
				})
				// console.log('ruleArr',ruleArr)
				rule = ruleArr.join('/')
				
				// return
				
				uni.request({
					url: "http://www.kangliuyong.com:10002/addShopcart",
					method: "POST",
					data: {
						pid: this.pid,
						count: this.num,
						rule: rule,
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						tokenString: this.token
					},

					header: {
						'content-type': 'application/x-www-form-urlencoded;charset=utf-8' // 默认值
					},
					success: (res) => {
						console.log("res", res)
						
						if(buy==true&&res.data.code==3000){
							uni.navigateTo({
								url: "/pages/pay/pay?sid="+ JSON.stringify([res.data.sid])
							})
							
							return
						}
						
						if(res.data.code==3000){
							uni.showToast({
								title:"加入购物车成功"
							})
						}
					},
					fail: (err) => {
						console.log("err", err)
					}
				})
			},

			gobuy() {
				if (!this.token) {
					uni.navigateTo({
						url: "/pages/login/login"
					})
					return
				}

				this.addCar(true)

				// uni.navigateTo({
				// 	url: "/pages/pay/pay"
				// })
			},

			goBack() {
				uni.navigateBack({
					delta: 1 //返回上一级
				})
			},

			add() {
				this.num++
			},

			minus() {
				if (this.num == 1) {
					//提示信息
					uni.showToast({
						title: "不能再减了",
						icon: "error"
					})
					return
				}
				this.num--
			},

			changeIndex(index, count) {
				this.typeData[index].index = count;

				// console.log('this.detailData.typeData',this.detailData.typeData)

				// this.$set(this.detailData.typeData[index],'index',count)
			},

			getDetail(pid) {
				uni.request({
					url: "http://www.kangliuyong.com:10002/productDetail",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						pid: pid
					},
					success: (res) => {
						console.log('res', res)
						this.detailData = res.data.result[0]
						console.log("this.detailData", this.detailData)

						res.data.result[0].desc = res.data.result[0].desc.split(/\n/)

						let typeAll = ['cream', 'milk', 'sugar', 'tem']
						let typeData = []
						typeAll.map(item => {
							let obj = {}
							obj.index = 0
							obj.type = res.data.result[0][item + '_desc']
							obj.typeValue = res.data.result[0][item].split('/');
							console.log('item', item)

							if (obj.typeValue[0].length != 0) {
								typeData.push(obj)
							}
						})

						console.log("typeData", typeData)

						this.typeData = typeData;

						this.detailData = res.data.result[0]
					},
					fail: (err) => {
						console.log('err', err)
					},

				})
			},
		}
	}
</script>

<style lang="scss">
	.detail {
		padding: bottom 180rpx;
		;

		.nav-top {
			position: fixed;
			z-index: 9999999999999;
			top: 0;
			width: 100%;
			height: 80rpx;
			background: white;
			text-align: center;
			// line-height: 80rpx;

			// image {
			// 	width: 30rpx;
			// 	height: 30rpx;
			// }

			// .go-back {
			// 	position: absolute;
			// 	top: 0;
			// 	left: 30rpx;
			// }
		}

		.detail-img {
			margin-top: 80rpx;

			image {
				width: 100%;
			}
		}

		.center-content {
			margin: 0 20rpx;
			//提高层级
			position: relative;
			z-index: 100;
			margin-top: -50rpx;

			// height: 800rpx;
			background: white;
			border-radius: 20rpx 20rpx 0 0;
			padding: 36rpx 26rpx;

			.title-price {
				margin-left: 16rpx;
				display: flex;
				justify-content: space-between;

				.title {
					font-weight: 600;
					color: #646566;
				}

				.price {
					margin-right: 16rpx;
					font-weight: 600;
					color: #0c34ba;
				}
			}

			.en-name {
				margin-left: 16rpx;
				margin-top: 8rpx;
				color: #a7b0b7;
				font-size: 24rpx;
			}

			.type-box {
				display: flex;
				margin-top: 20rpx;
				align-items: center;

				.type {
					margin-left: 16rpx;
					width: 100rpx;
					font-size: 28rpx;
					color: #817d76;
				}

				text {
					margin-left: 30rpx;
					padding: 10rpx;
					border-radius: 30rpx;
					margin-right: -10rpx;
					background: #e8e8e8;
					display: inline-block; //转换成行内块 设置宽度
					width: 120rpx;
					text-align: center;
					font-size: 26rpx;
					color: #686769;
					justify-content: space-between;
				}

				.active {
					background: #0c34ba;
					color: white;
				}
			}

			.choose-count {
				margin: 16rpx;
				margin-top: 40rpx;
				height: 130rpx;
				display: flex;
				align-items: center;
				justify-content: space-between;
				border-top: 1rpx solid #80808047;
				border-bottom: 1px solid #80808047;
				font-size: 26rpx;

				.choose-text {
					color: #646566;
					font-size: 28rpx;
				}

				.btn-box {
					display: flex;
					align-items: center;

					.num {
						margin: 0 20rpx;
					}

					image {
						width: 40rpx;
						height: 40rpx;
					}
				}
			}

			.product-instr {
				.goods-des {
					color: #646566;
					font-size: 26rpx;
				}

				.goods-item {
					margin-top: 20rpx;
					color: #a09f9d;
					font-size: 24rpx;
				}
			}
		}

		.footer {
			position: fixed;
			z-index: 9999999999;
			bottom: 0;
			width: 100%;
			height: 100rpx;
			background: skyblue;
			display: flex;
			align-items: center;
			background: white;

			.footer-left {
				width: 200rpx;
				height: 100%;
				// background: yellowgreen;
				display: flex;
				font-size: 24rpx;
				text-align: center;
				align-items: center;
				justify-content: space-between;
				padding: 0 15rpx;
				box-sizing: border-box;
				color: #646566;

				image {
					width: 40rpx;
					height: 40rpx;
				}
			}

			.footer-right {
				// width: 400rpx;
				flex: 1;
				height: 80rpx;
				// background: white;
				display: flex;
				line-height: 76rpx;
				text-align: center;
				font-size: 30rpx;
				color: white;
				padding: 10rpx;

				.add-car {
					width: 50%;
					height: 100%;
					background: #6a91ec;
					border-radius: 40rpx 0 0 40rpx;
				}

				.go-buy {
					width: 50%;
					height: 100%;
					background: #0c34ba;
					border-radius: 0 40rpx 40rpx 0;
				}
			}
		}
	}
</style>