<template>
	<view style="position:relative;">
		<view class="back">
			<image src="../../static/arrow.png" mode="" style="width:60rpx;height:60rpx;" @click="back"></image>
		</view>
		<view style="display:flex;justify-content:center;">
			<view>
				<image src="../../static/head.jpg" mode="" style="width:200rpx;height:200rpx;border-radius:50%;padding-top:100rpx;"></image>
			</view>
		</view>
		<view style="margin-top:200rpx">
			<form @submit="onSubmit">
				<view style="display:flex;justify-content:space-between;align-items:center;border-bottom:2rpx solid #eee;margin:40rpx 100rpx;">
					<view style="font-size: 36rpx;">账号</view>
					<view>
						<input name="input" placeholder="请输入用户名" v-model="username" style="padding:10rpx;" />
					</view>
				</view>
				<view style="display:flex;justify-content:space-between;align-items:center;border-bottom:2rpx solid #eee;margin:40rpx 100rpx;">
					<view style="font-size:36rpx;">密码</view>
					<view>
						<input name="input" placeholder="请输入密码" v-model="password" style="padding:10rpx;" />
					</view>
				</view>
				<button form-type="submit" type="primary">登录/注册</button>
			</form>
		</view>
		<view>
			<view style="display:flex;justify-content:center;color:#ccc;padding-top:50rpx">
				第三方账号登录
			</view>
			<view style="display:flex;justify-content:center;padding-top:50rpx">
				<view style="display:flex;justify-content:space-between;width:300rpx">
					<view>
						<image src="../../static/weixin.png" mode="" style="width:50rpx;height:50rpx;" @click="weixin"></image>
					</view>
					
					<view>
						<!-- #ifdef MP-WEIXIN -->
						<image src="../../static/QQ.png" mode="" style="width:50rpx;height:50rpx;" @click="QQ"></image>
						<!-- #endif -->
					</view>
						
					<view>
						<image src="../../static/weibo.png" mode="" style="width:50rpx;height:50rpx;" @click="weibo"></image>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '', //用户名
				password: '' ,//密码
				login:{}//登录返回数据
			}
		},
		methods: {
			//返回
			back() {
				uni.navigateBack()
			},
			// 登录注册
			onSubmit(e) {
				console.log(e)
				uni.request({
						url: `${this.$api}/user/registOrLogin?qq=40699904`,
						method: 'POST',
						data: {userBO:'qq',username:this.username,password:this.password},
						success: res => {
							if (res.data.status === 200) {
								uni.hideLoading()
								uni.showToast({
								    title: '登录成功',
									icon:'success',
								    duration: 2000
								});
								this.login = res.data.data
								localStorage.setItem('login',JSON.stringify(this.login))
								uni.switchTab({
								    url: '../index/index'
								});
								console.log(res, "登录注册");
							}
						},
						fail: () => {},
						complete: () => {}
					});
			},
			//微信登录
			weixin(e){
				console.log(e,"weixin")
				uni.login({
				  provider: 'weixin',
				  success: function (loginRes) {
				    console.log(loginRes,"loginRes");
				    // 获取用户信息
				    uni.getUserInfo({
				      provider: 'weixin',
				      success: function (infoRes) {
						  console.log(infoRes,"infoRes");
				        console.log('用户昵称为：' + infoRes.userInfo.nickName);
				      }
				    });
				  }
				});
			},
			//qq登录
			//#ifndef MP-WEIXIN
			QQ(e){
				console.log(e,"QQ")
				uni.login({
				  provider: 'qq',
				  success: function (loginRes) {
				    console.log(loginRes,"loginRes");
				    // 获取用户信息
				    uni.getUserInfo({
				      provider: 'qq',
				      success: function (infoRes) {
						  console.log(infoRes,"infoRes");
				        console.log('用户昵称为：' + infoRes.userInfo.nickName);
				      }
				    });
				  }
				});
			},
			//#endif
			//微博登录
			weibo(e){
				console.log(e,"weibo")
				uni.login({
				  provider: 'sinaweibo',
				  success: function (loginRes) {
				    console.log(loginRes,"loginRes");
				    // 获取用户信息
				    uni.getUserInfo({
				      provider: 'sinaweibo',
				      success: function (infoRes) {
						  console.log(infoRes,"infoRes");
				        console.log('用户昵称为：' + infoRes.userInfo.nickName);
				      }
				    });
				  }
				});
			},
			onLoad(options){
				
			}
		}
	}
</script>

<style>
	.back {
		width: 80rpx;
		height: 80rpx;
		background-color: rgba(0, 0, 0, 0.3);
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		left: 40rpx;
		top: 40rpx;
	}
</style>
