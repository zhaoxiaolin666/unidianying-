<template>
	<view>
		<!-- 未登录 -->
		<view class="nologin" v-if="!flag">
			<view>
				<image src="../../static/head.jpg" mode="" style="width:100rpx;height:100rpx;border-radius:50%;margin:100rpx 100rpx 0 40rpx"></image>
			</view>
			<view style="font-weight:600;font-size:48rpx;margin-top:126rpx;color:#333;" @click="goto">
				登录/注册
			</view>
		</view>
		<!-- 已登录 -->
		<view class="nologin" v-else>
			<view>
				<image :src="login.faceImage" mode="" style="width:100rpx;height:100rpx;border-radius:50%;margin:100rpx 100rpx 0 40rpx"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				flag:false,//登录标记
				login:{}//登录本地数据
			}
		},
		methods: {
			goto(){
				//在起始页面跳转到logined.vue页面
				uni.navigateTo({
				    url: '../logined/logined'
				});
			}
		},
		onLoad(options){
			if(JSON.parse(localStorage.getItem('login'))){
				this.flag=true
				this.login=JSON.parse(localStorage.getItem('login'))
			}else{
				this.flag=false
			}
		}
	}
</script>

<style>
.nologin{
	background-color: skyblue;
	width:100%;
	height:300rpx;
	display: flex;
}
</style>
