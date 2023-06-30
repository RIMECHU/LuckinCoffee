<template>
	<view class="index">
		<view class="search-box">
			<view class="time-name">
				<text class="time">{{getTime()}}</text>
				<text class="name">Allen</text>
			</view>
			<view class="search-input">
				<image src="../../static/search_icon.png" mode=""></image>
				<input type="text" placeholder="输入商品名称">
			</view>
		</view>
		<view class="banner">
			<swiper class="swiper" circular autoplay interval="2000" duration="1000"
				@change="changeSwiper">
				<swiper-item v-for="(item,index) in bannerData" :key="item.pid" @click="toDetail(item.pid)">
					<image :src="item.bannerImg" mode="widthFix"></image>
				</swiper-item>
			</swiper>
			
			<view class="doints">
				<view class="item" :class="{active:index==0}"></view>
				<view class="item" :class="{active:index==1}"></view>
				<view class="item" :class="{active:index==2}"></view>
				<view class="item" :class="{active:index==3}"></view>
			</view>
		</view>
		
		<view class="rec">
			<view class="rec-text">热卖推荐</view>
		</view>
		
		<view class="product-content">
			<view class="product-item" v-for="(item,index) in ProductData" :key="item.pid" @click="toDetail(item.pid)">
				<view class="img-box" >
					<image :src="item.smallImg" mode="widthFix"></image>
					<text>hot</text>
				</view>
				
				<view class="pro-name">{{item.name}}</view>
				<view class="en-name">{{item.enname}}</view>
				<view class="price">￥{{item.price}}</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				bannerData: [],
				index:0,
				ProductData:[],
			}
		},
		onLoad() {
			this.getData();
			this.getProduct();
		},
		methods: {
			getTime(){
				let timeNow = new Date();
				let hours = timeNow.getHours();
				let state='';
				if (hours >= 0 && hours <= 10) {
				        state = '早上好';
				    } else if (hours > 10 && hours <= 14) {
				        state= '中午好';
				    } else if (hours > 14 && hours <= 18) {
				        state= '下午好';
				    } else if (hours > 18 && hours <= 24) {
				        state= '晚上好';
				    }
				    return state;
			},
			getProduct(){
				uni.request({
					url: 'http://www.kangliuyong.com:10002/typeProducts', //仅为示例，并非真实接口地址。
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						key: 'isHot',
						value: 1
					},
					// header: {
					//     'custom-header': 'hello' //自定义请求头信息
					// },
					success: (res) => {
						// console.log(res.data);
						// this.text = 'request success';
						this.ProductData = res.data.result
						console.log("this.ProductData", this.ProductData)
					},
					fail: (err) => {
						console.log('err', err);
					}
				});
			},
			
			getData() {
				uni.request({
					url: 'http://www.kangliuyong.com:10002/banner', //仅为示例，并非真实接口地址。
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA='
					},
					// header: {
					//     'custom-header': 'hello' //自定义请求头信息
					// },
					success: (res) => {
						// console.log(res.data);
						// this.text = 'request success';
						this.bannerData = res.data.result
						console.log("this.bannerData", this.bannerData)
					},
					fail: (err) => {
						console.log('err', err);
					}
				});

			},
			changeSwiper(e) {
				// console.log('e', e.detail.current)
				this.index=e.detail.current;

			},
			toDetail(pid){
				uni.navigateTo({
					url:"/pages/detail/detail?pid="+pid
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background: #f6f6f6;
	}

	.index {
		.search-box {
			padding: 0 30rpx;
			height: 100rpx;
			background: white;
			display: flex;
			align-items: center;
			justify-content: space-between;
		}

		.time {
			font-weight: 600;
			font-size: 26rpx;
			color: #746e69;
		}

		.name {
			margin-left: 20rpx;
			font-weight: 600;
			font-size: 26rpx;
			color: #0c35ba;
		}

		.search-input {
			padding: 0 20rpx;
			height: 80rpx;
			display: flex;
			align-items: center;
			background: #f7f8fa;
			border-radius: 50rpx;
			color: #dbd6d4;

			image {
				margin-right: 10rpx;
				width: 35rpx;
				height: 35rpx;
			}

			input {
				font-size: 30rpx;
			}
		}

		.banner {
			margin: 30rpx;
			border-radius: 12rpx;
			overflow: hidden;
			position: relative;

			.swiper {
				height: 514rpx;
			}

			image {
				width: 100%;
			}

			.doints {
				width: 180rpx;
				height: 30rpx;
				position: absolute;
				// background: pink;
				// margin-top: 0rpx;
				bottom: 20rpx;
				right: 20rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;

				.item {
					width: 30rpx;
					height: 5rpx;
					background: white;
				}
				
				.active{
					background: #0c35ba;
				}
			}
		}
		
		.rec{
			margin: 30rpx;
			height: 100rpx;
			background: white;
			overflow: hidden;
			
			.rec-text{
				margin-top: 15rpx;
				width: 200rpx;
				height: 70rpx;
				background: #0c35ba;
				color: white;
				text-align: center;
				line-height: 70rpx;
				border-top-right-radius: 30rpx;
			}
		}
		
		.product-content{
			display: flex;
			flex-wrap: wrap;
			margin: 0 30rpx;
			justify-content: space-between;
			
			.product-item{
				padding: 20rpx;
				box-sizing: border-box; //怪异盒子模型
				margin-bottom: 30rpx;
				width: 48%;
				height: 500rpx;
				background: white;
				border-radius: 15rpx;
				
				.img-box{
					position: relative;
				}
				.img-box image{
					width: 100%;
					border-radius: 10rpx;
				}
				.img-box text{
					padding:10rpx 6rpx;
					position: absolute;
					left: 20rpx;
					top: 0;
					background: #0c35ba;
					color: white;
					font-size: 26rpx;
					border-radius: 0 0 10rpx 10rpx;
				}
				
				.pro-name{
					margin: 8rpx 0;
					color: #828466;
					font-size: 34rpx;
				}
				
				.en-name{
					overflow: hidden;
					white-space: nowrap;
					text-overflow: ellipsis;
					color: #b3a7a5;
					font-size: 26rpx;
				}
				
				.price{
					margin-top: 15rpx;
					color: #0c35ba;
					font-weight: 600;
					font-size: 34rpx;
				}
			}
		}
	}
</style>