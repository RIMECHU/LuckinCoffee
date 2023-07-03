<template>
	<view class="login">
		<view class="login-nav">
			<view class="nav-icon-text">
				<image src="../../static/home_active.png" mode=""></image>
				<text>Luckin Coffee</text>
			</view>

			<view class="gohome" @click="toIndex">先逛一逛</view>
		</view>

		<view class="welcome-back">
			<view class="welcome">欢迎回来!</view>
			<view class="welcome-en">Please login to your accounts</view>
		</view>

		<view class="form">
			<view class="same-input">
				<text>手机号</text>
				<input type="text" placeholder="手机号" v-model="loginData.phone">
			</view>
			<view class="same-input">
				<text>密码</text>
				<input class="psd-input" :type="showPassword?'password':'text'" placeholder="密码必须为字母开头" v-model="loginData.password">

				<view class="icons" @click="showPassword=!showPassword">
					<uni-icons :type="showPassword?'eye-slash':'eye'" size="20"></uni-icons>
				</view>

			</view>

			<view class="forget-psd">忘记密码?</view>
		</view>

		<view class="btn-login" @click="login">登录</view>
		<view class="btn-res" @click="open">注册</view>

		<uni-popup class="popup" ref="popup" background-color="#fff" type="bottom">
			<view class="res-title">
				<text>注册</text>
				<uni-icons type="closeempty" size="25" color="#c9ced7" @click="close"></uni-icons>
			</view>
			<view class="form">
				<view class="same-input">
					<text>手机号</text>
					<input type="text" placeholder="11位手机号" v-model="resData.phone" />
				</view>

				<view class="same-input">
					<text>密码</text>
					<input class="psd-input" :type="showPassword?'password':'text'" placeholder="密码必须为字母开头"
						v-model="resData.password" />

					<view class="icons" @click="showPassword=!showPassword">
						<uni-icons :type="showPassword?'eye-slash':'eye'" size="20"></uni-icons>
					</view>
				</view>

				<view class="same-input">
					<text>昵称</text>
					<input type="text" placeholder="昵称" v-model="resData.nickName" />
				</view>
			</view>
			<view class="btn-login" @click="register">注册</view>
		</uni-popup>


	</view>
</template>

<script>
	//@ :定位项目
	import {
		validForm
	} from '@/static/validForm.js'

	export default {
		data() {
			return {
				resData: {
					phone: "",
					password: "",
					nickName: ""
				},
				loginData: {
					phone: "",
					password: "",
				},
				showPassword: true
			}
		},
		methods: {
			toIndex(){
				uni.switchTab({
					url:"/pages/index/index"
				})
			},
			
			login() {
				console.log("this.loginData", this.loginData);
				
				let o = {
					phone: {
						value: this.loginData.phone,
						errorMsg: '手机号格式不正确',
						reg: /^1[3-9]\d{9}$/
					},
					password: {
						value: this.loginData.password,
						errorMsg: '密码由数字字母下划线组合(6-16字符)',
						reg: /^[A-Za-z]\w{5,15}$/
					},
				};
				
				let isPass = validForm.valid(o); //返回一个true 或 false 进行判断
				
				// console.log("isPass",isPass)
				
				if (!isPass) {
					return
				}
				
				uni.request({
					url: "http://www.kangliuyong.com:10002/login",
					method: "POST",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						password: this.loginData.password,
						phone: this.loginData.phone
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset=utf-8' // 默认值
					},
					success: (res) => {
						console.log('res', res)
						
						if(res.data.code == 200){
							uni.showToast({
								title:"登录成功"
							})
							
							uni.setStorageSync("token",res.data.token)
							
							uni.navigateBack({
								delta:1
							})
						}else{
							uni.showToast({
								title:res.data.msg,
								icon:"error"
							})
						}
					},
					fail: (err) => {
						console.log('err', err)
					},
				
				})
			},

			open() {
				// 通过组件定义的ref调用uni-popup方法 ,如果传入参数 ，type 属性将失效 ，仅支持 ['top','left','bottom','right','center']
				this.$refs.popup.open()
			},
			close() {
				this.$refs.popup.close()
			},

			register() {
				console.log("this.resData", this.resData)

				//正则
				let o = {
					phone: {
						value: this.resData.phone,
						errorMsg: '手机号格式不正确',
						reg: /^1[3-9]\d{9}$/
					},
					password: {
						value: this.resData.password,
						errorMsg: '密码由数字字母下划线组合(6-16字符)',
						reg: /^[A-Za-z]\w{5,15}$/
					},
					nickName: {
						value: this.resData.nickName,
						errorMsg: '昵称由字母数字下划线汉字组合(1-10字符)',
						reg: /^[\w\u4e00-\u9fa5]{1,10}$/
					},
				};

				let isPass = validForm.valid(o); //返回一个true 或 false 进行判断

				// console.log("isPass",isPass)

				if (!isPass) {
					return
				}

				// return

				uni.request({
					url: "http://www.kangliuyong.com:10002/register",
					method: "POST",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						nickName: this.resData.nickName,
						password: this.resData.password,
						phone: this.resData.phone
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset=utf-8' // 默认值
					},
					success: (res) => {
						console.log('res', res)
					},
					fail: (err) => {
						console.log('err', err)
					},

				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		background: white;
	}

	.login {
		.login-nav {
			padding: 0 30rpx;
			height: 100rpx;
			background: white;
			display: flex;
			justify-content: space-between;
			align-items: center;
			border-bottom: 1rpx solid ghostwhite;

			.nav-icon-text {
				display: flex;
				align-items: center;

				image {
					margin-right: 10rpx;
					width: 66rpx;
					height: 66rpx;
				}

				text {
					font-weight: 600;
					color: #646566;
				}
			}

			.gohome {
				color: #0c34c9;
				font-weight: 600;
				font-size: 28rpx;
			}
		}

		.welcome-back {
			margin-top: 200rpx;
			margin-left: 30rpx;

			.welcome {
				font-weight: 600;
				font-size: 58rpx;
				color: #646566;
			}

			.welcome-en {
				margin-top: 30rpx;
				color: #646566;
			}
		}

		.form {
			margin-top: 60rpx;

			.same-input {
				margin: 0 36rpx;
				height: 100rpx;
				display: flex;
				align-items: center;
				border-bottom: 1rpx solid ghostwhite;
				position: relative;

				text {
					width: 200rpx;
					font-size: 26rpx;

				}

				input {
					font-size: 26rpx;
				}

				.psd-input {
					border: none !important;
					outline: none !important;
				}

				.icons {
					position: absolute;
					right: 0;
				}
			}

			.forget-psd {
				margin-top: 20rpx;
				margin-right: 30rpx;
				float: right;
				font-size: 26rpx;
			}
		}

		.btn-login,
		.btn-res {
			margin: 0 50rpx;
			height: 80rpx;
			margin-top: 150rpx;
			background: #0c34ba;
			color: white;
			text-align: center;
			line-height: 80rpx;
			border-radius: 50rpx;

		}

		.btn-res {
			margin-top: 100rpx;
			background: white;
			color: black;
			border: 1rpx solid #80808063;
		}

		.popup {
			.res-title {
				padding: 30rpx;
				display: flex;
				justify-content: space-between;

				text {
					font-weight: 600;
					font-size: 45rpx;
					color: #646566;
				}
			}

			.form {
				color: #646566;
			}

			.btn-login {
				margin-top: 80rpx;
				margin-bottom: 50rpx;
			}
		}
	}
</style>