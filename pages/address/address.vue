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
					<u--input v-model="form.phone" border="none" placeholder="手机"></u--input>
				</u-form-item>

				<u-form-item @click="show=!show" label="地区" borderBottom labelWidth="140rpx" prop="userInfo.name"
					borderBottom ref="item1">
					<u--input class="input-area" v-model="form.area" disabled border="none" placeholder="地区"></u--input>
					<u-icon slot="right" name="arrow-right"></u-icon>
				</u-form-item>

				<u-form-item label="详细地址" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom
					ref="item1">
					<u--input v-model="form.detailArea" border="none" placeholder="详细地址"></u--input>
				</u-form-item>

				<u-form-item label="邮政编码" borderBottom labelWidth="140rpx" prop="userInfo.name" borderBottom
					ref="item1">
					<u--input v-model="form.post" border="none" placeholder="邮政编码"></u--input>
				</u-form-item>
				<!-- <u-form-item label="性别" prop="userInfo.sex" borderBottom @click="showSex = true; hideKeyboard()"
					ref="item1">
					<u--input v-model="model1.userInfo.sex" disabled disabledColor="#ffffff" placeholder="请选择性别"
						border="none"></u--input>
					<u-icon slot="right" name="arrow-right"></u-icon>
				</u-form-item> -->
			</u--form>

			<u-picker :show="show" ref="uPicker" :columns="cityList" @confirm="confirm" @change="changeHandler"
				@cancel="show = false"></u-picker>
		</view>

		<view class="default-btn">
			<text>设置默认收货地址</text>
			<u-switch v-model="value" size="25"></u-switch>
		</view>

		<view class="btn-save" @click="add">保存地址</view>
		<view class="btn-del" @click="del">删除</view>

	</view>
</template>

<script>
	// 导入城市js文件
	import cityData from './city.js'

	export default {
		data() {
			return {
				//显示选择器
				show: false,
				// 打开选择器显示默认城市
				cityList: [],


				cityLevel1: [],
				cityLevel2: [],
				cityLevel3: [],
				form: {
					name: "",
					phone: "",
					area: "",
					detailArea: "",
					post: "",
					default: 0,
					
				},
				value: false,
				token:"",
				province:"",
				city:"",
				county:""
			}
		},
		onLoad() {
			// 城市选择器初始化
			this.initCityData();
		},
		methods: {



			goBack() {
				uni.navigateBack({
					delta: 1 //返回上一级
				})
			},

			initCityData() {
				// 遍历城市js
				cityData.forEach((item1, index1) => {
					let temp2 = [];
					this.cityLevel1.push(item1.provinceName);

					let temp4 = [];
					let temp3 = [];
					// 遍历市
					item1.cities.forEach((item2, index2) => {
						temp2.push(item2.cityName);
						// 遍历区
						item2.counties.forEach((item3, index3) => {
							temp3.push(item3.countyName);
						})
						temp4[index2] = temp3;
						temp3 = [];
					})
					this.cityLevel3[index1] = temp4;

					this.cityLevel2[index1] = temp2;
				})
				// 选择器默认城市
				this.cityList.push(this.cityLevel1, this.cityLevel2[0], this.cityLevel3[0][0]);
			},
			// 选中时执行
			changeHandler(e) {
				const {
					columnIndex,
					index,
					indexs,
					value,
					values,
					// 微信小程序无法将picker实例传出来，只能通过ref操作
					picker = this.$refs.uPicker
				} = e;
				if (columnIndex === 0) { // 选择第一列数据时
					// 设置第二列关联数据
					picker.setColumnValues(1, this.cityLevel2[index]);
					// 设置第三列关联数据
					picker.setColumnValues(2, this.cityLevel3[index][columnIndex]);
				} else if (columnIndex === 1) { // 选择第二列数据时
					// 设置第三列关联数据
					picker.setColumnValues(2, this.cityLevel3[indexs[0]][index]);
				}
			},
			// 单击确认按钮时执行
			confirm(e) {
				// 输出数组 [省, 市, 区]
				console.log(e.value);
				
				this.province = e.value[0]
				this.city = e.value[1]
				this.county = e.value[2]
				
				this.form.area = e.value.join('/')
				// 隐藏城市选择器
				this.show = false;
			},
			
			onShow(){
				this.token = uni.getStorageSync('token')
			},

			add() {
				// console.log("form", form)
				
				if (!this.token) {
					uni.navigateTo({
						url: "/pages/login/login"
					})
					return
				}
				
				// return
				
				

				uni.request({
					url: "http://www.kangliuyong.com:10002/addAddress",
					method: "POST",
					data: {
						appkey: 'U2FsdGVkX19WSQ59Cg+Fj9jNZPxRC5y0xB1iV06BeNA=',
						tokenString: this.token,
						name: this.form.name,
						tel: this.form.phone,
						province: this.province,
						city: this.city,
						county: this.county,
						addressDetail: this.form.detailArea,
						// areaCode: 地区编号,
						postalCode: this.form.post,
						isDefault: this.value ? 1:0
					},
					
					 header: {
					 'content-type': 'application/x-www-form-urlencoded;charset=utf-8' // 默认值
					 },
					
					success: (res) => {
						console.log("res", res)
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
	.address {
		.nav {
			/deep/ .uni-icons {
				color: #0c34ba !important;
			}

			/deep/ .uni-navbar-btn-text span {
				color: #0c34ba !important;
			}


		}

		.content {
			margin: 30rpx;
			background: white;
			border-radius: 20rpx;
			padding: 0 30rpx;

			/deep/ .u-form-item__body__left__content__label {
				color: #646566 !important;
				font-size: 28rpx;
			}

			/deep/ .uni-input-placeholder {
				background: white;
			}
			
			// .input-area{
			// 	/deep/ .uni-input-input {
			// 		background: white;
			// 	}
			// }
		}

		.default-btn {
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