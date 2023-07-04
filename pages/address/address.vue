<template>
	<view class="address">
		<view class="nav">
			<uni-nav-bar :border="false" left-icon="left" left-text="返回" @clickLeft="goBack" :fixed="true"
				title="编辑地址"></uni-nav-bar>
		</view>

		<view class="content">
			<u--form labelPosition="left" :model="form" ref="uForm">
				<u-form-item label="姓名" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom ref="item1">
					<u--input v-model="form.name" border="none" placeholder="姓名"></u--input>
				</u-form-item>
				
				<u-form-item label="手机" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom ref="item1">
					<u--input v-model="form.name" border="none" placeholder="手机"></u--input>
				</u-form-item>
				
				<u-form-item label="地区" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom ref="item1">
					<u--input v-model="form.name" border="none" placeholder="地区"></u--input>
					<u-icon slot="right" name="arrow-right"></u-icon>
				</u-form-item>
				
				<u-form-item label="详细地址" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom ref="item1">
					<u--input v-model="form.name" border="none" placeholder="详细地址"></u--input>
				</u-form-item>
				
				<u-form-item label="邮政编码" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom ref="item1">
					<u--input v-model="form.name" border="none" placeholder="邮政编码"></u--input>
				</u-form-item>
				<!-- <u-form-item label="性别" prop="userInfo.sex" borderBottom @click="showSex = true; hideKeyboard()"
					ref="item1">
					<u--input v-model="model1.userInfo.sex" disabled disabledColor="#ffffff" placeholder="请选择性别"
						border="none"></u--input>
					<u-icon slot="right" name="arrow-right"></u-icon>
				</u-form-item> -->
			</u--form>
		</view>
		
		<view class="default-btn">
			<text>设置默认收货地址</text>
			<u-switch v-model="value" size="25"></u-switch>
		</view>
		
		<view class="btn-save" @click="save">保存地址</view>
		<view class="btn-del" @click="del">删除</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form:{
					name:""
				},
				value:false
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
	.address {
		.nav {
			/deep/ .uni-icons {
				color: #0c34ba !important;
			}

			/deep/ .uni-navbar-btn-text span {
				color: #0c34ba !important;
			}
		}
		
		.content{
			margin: 30rpx;
			background: white;
			border-radius: 20rpx;
			padding: 30rpx;
			
			/deep/ .u-form-item__body__left__content__label{
				color: #646566 !important;
				font-size: 28rpx;
			}
		}
		
		.default-btn{
			margin: 30rpx;
			padding: 30rpx;
			box-sizing: border-box;
			height: 100rpx;
			background-color: white;
			border-radius: 20rpx;
			font-size: 28rpx;
			
			display: flex;
			justify-content: space-between;
			align-items: center;
		}
		
		.btn-save,
		.btn-del {
			margin: 0 30rpx;
			height: 90rpx;
			margin-top: 60rpx;
			background: #0c34ba;
			color: white;
			text-align: center;
			line-height: 90rpx;
			border-radius: 50rpx;
			font-size: 28rpx;
		}
		
		.btn-del {
			margin-top: 25rpx;
			background: white;
			color: black;
			border: 1rpx solid #80808063;
		}
	}
</style>