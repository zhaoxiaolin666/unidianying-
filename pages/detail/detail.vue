<template>
	<view class="box">
		<!-- 返回 -->
		<view class="back">
			<image src="../../static/arrow.png" mode="" style="width:60rpx;height:60rpx;" @click="back"></image>
		</view>
		<!-- 分享 -->
		<view class="share">
			<image src="../../static/share.png" mode="" style="width:60rpx;height:60rpx;" @click="share"></image>
		</view>
		<!-- 视频 -->
		<view>
			<video :src="detail.trailer" style="width:100%;height:500rpx;" :poster="detail.cover"></video>
		</view>
		<!-- 详情 -->
		<view style="display:flex;justify-content:start;padding:50rpx 10rpx;padding-bottom:100rpx;">
			<view>
				<image :src="detail.cover" mode="" style="width:250rpx;height:300rpx;padding-right:20rpx;"></image>
			</view>
			<view>
				<view style="font-size:40rpx;">
					{{detail.name}}
				</view>
				<view style="font-size:28rpx;color:#666;">
					{{detail.basicInfo}}
				</view>
				<view style="font-size:28rpx;color:#666;">
					{{detail.originalName}}
				</view>
				<view style="font-size:28rpx;color:#666;">
					{{detail.releaseDate}}
				</view>
				<view style="display:flex;justify-content: space-between;">
					<view>
						<view style="font-size:36rpx;">
							综合评分
						</view>
						<view style="padding-left:100rpx;color:orange;font-weight:500;">
							{{detail.score}}
						</view>
					</view>
					<view>
						<view style="font-size:28rpx;color:#666;margin-top:50rpx;">
							<uni-rate :value="detail.score/2" color="#bbb" active-color="orange" :size="16" :readonly="true"/>
							{{detail.prisedCounts}}人点赞
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 详情描述 -->
		<view style="border-top:1rpx solid #ddd;margin:10rpx">
			<view style="font-size:28rpx;color:#666;padding:10rpx 0;">
				剧情介绍
			</view>
			<view style="font-size:28rpx;border-bottom:1rpx solid #ddd;padding:10rpx 0;">
				{{detail.plotDesc}}
			</view>
			<view style="font-size:28rpx;color:#666;padding:10rpx 0;">
				演职人员
			</view>
			<view>
				<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120" :show-scrollbar="true">
					<view class="scroll-view-item_H" v-for="(item,index) in role" :key="item.id">
						<image :src="item.photo" mode="" style="width:170rpx;height:200rpx;" ></image>
						<view class="name" style="font-size: 28rpx;width: 170rpx;">{{item.name}}</view>
						<view v-if="index===0" style="font-size:28rpx;color:#ccc;">
							导演
						</view>
						<view v-else class="name" style="font-size:28rpx;color:#ccc;">
							饰演{{item.actName}}
						</view>
					</view>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				detail: {}, //详情电影
				role:[]//演职人员
			}
		},
		methods: {
			//返回
			back() {
				uni.navigateBack()
			},
			//分享
			share() {
				uni.showToast({
						title: '此功能正在开发,敬请期待',
						icon: 'none'
					});
			},
			//演职人员
			postroles(){
				uni.request({
					url: `${this.$api}/search/staff/${this.detail.id}/${1}?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							this.role = res.data.data
							console.log(res.data.data, "导演");
							uni.request({
								url: `${this.$api}/search/staff/${this.detail.id}/${2}?qq=40699904`,
								method: 'POST',
								data: {},
								success: res => {
									if (res.data.status === 200) {
										uni.hideLoading()
										this.role = this.role.concat(res.data.data)
										console.log(res.data.data, "演职人员");
									}
								},
								fail: () => {},
								complete: () => {}
							});
										
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
		},
		//页面加载
		onLoad(options) {
			uni.showLoading({
				title: "加载中..."
			})
			this.detail = JSON.parse(options.detail)
			console.log(this.detail);
			console.log(options, "详情options");
			this.postroles()
		}
	}
</script>

<style>
	.box {
		position: relative;
	}

	.back {
		width: 80rpx;
		height: 80rpx;
		background-color: rgba(0, 0, 0, 0.3);
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		left: 10rpx;
		top: 10rpx;
		z-index: 5;
	}

	.share {
		width: 80rpx;
		height: 80rpx;
		background-color: rgba(0, 0, 0, 0.3);
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		right: 10rpx;
		top: 10rpx;
		z-index: 5;
	}
	.scroll-view_H {
		white-space: nowrap;
	}
	
	.scroll-view-item_H {
		display: inline-block;
		width: 170rpx;
		height:200rpx;
		padding-right: 20rpx
	}
	
	.name {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
</style>
