<template>
	<view class="menu">
		<view class="search-box">
			<view class="search-input">
				<image src="../../static/search_icon.png" mode=""></image>
				<input type="text" placeholder="输入商品名称">
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
	.menu{
		.search-box {
			padding: 0 30rpx;
			height: 100rpx;
			background: white;
			display: flex;
			align-items: center;
			justify-content: space-between;
		}
		
		.search-input {
			padding: 0 20rpx;
			width: 100%;
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
	}
</style>
