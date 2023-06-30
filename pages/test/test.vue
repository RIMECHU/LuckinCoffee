<template>
	<view class="test">
		<button v-on:click="showMessage =! showMessage">显示/隐藏</button>
		<view v-show="showMessage">RIM</view>
		<view v-html="rawHtml"></view>
		<view>{{rawHtml}}</view>
		<button v-model="use" @click="N">修改颜色</button>
		<view v-bind:class="{'class1': use}">RIM</view>
		
		<view v-for="(value, name, index) in object">{{ index }}. {{ name }}: {{ value }}</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				object: {
							title: 'How to do lists in Vue',
							author: 'Jane Doe',
							publishedAt: '2020-04-10',
						},
				showMessage:true,
				rawHtml: '<span style="color: red">这里会显示红色！</span>',
				use:false,
				bannerData:[]
				
			}
		},
		onLoad() {
			this.getData();
		},
		methods: {
			N(){
				this.use = true
			},
			getData(){
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
						console.log("this.bannerData",this.bannerData)
				    },
					fail: (err) => {
						console.log('err',err);
					}
				});

			}
		}
	}
</script>

<style>
	.class1{
	  background: #444;
	  color: #eee;
	}
</style>
