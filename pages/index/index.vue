<template>
	<view>
		<!-- 轮播图 -->
		<view>
			<swiper indicator-dots="true" autoplay circular interval="5000" duration="1500" indicator-active-color="#fff"
			 indicator-color="rgba(0, 0, 0, .5)">
				<swiper-item v-for="item in banners" :key="item.id">
					<image :src="item.image" mode="" style="width:100%;height:400rpx"></image>
				</swiper-item>
			</swiper>
		</view>
		<!-- 热门超英 -->
		<view>
			<view style="padding:20rpx">
				<image src="../../static/fire.png" mode="" style="width:48rpx;height:48rpx" ></image>
				<text style="font-weight:600">热门超英</text>
			</view>
			<view style="padding-left:20rpx">
				<view>
					<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120" :show-scrollbar="true">
						<view class="scroll-view-item_H" v-for="(item,index) in premierpeague" :key="item.id">
							<image :src="item.cover" mode="" style="width:170rpx;height:200rpx;" @click="gotodetail(index)"></image>
							<view class="name" style="font-size: 28rpx;width: 170rpx;">{{item.name}}</view>
							<view style="display:flex;align-items:center;">
								<view style="padding-top:6rpx;">
									<uni-rate :value="item.score/2" :size="12" />
								</view>
								<view style="font-size:24rpx;">{{item.score}}</view>
							</view>
						</view>
					</scroll-view>
				</view>
			</view>
		</view>
		<!-- 热门预告 -->
		<view>
			<view style="padding:20rpx">
				<image src="../../static/Notice.png" mode="" style="width:48rpx;height:48rpx;"></image>
				<text style="font-weight:600;">热门预告</text>
			</view>
			<view style="display:flex;justify-content:space-around;flex-wrap:wrap;">
				<view style="padding:20rpx" v-for="item in notice" :key="item.id">
					<video :src="item.trailer" style="width:320rpx;height:400rpx;" :poster="item.cover"></video>
				</view>
			</view>
		</view>
		<!-- 猜我喜欢 -->
		<view style="padding:20rpx">
			<view style="display:flex;justify-content:space-between;margin-top:40rpx;" v-for="(item,index) in guessULike" :key="item.id">
				<view style="border-radius:6rpx;padding-right:20rpx;">
					<image :src="item.cover" mode="" style="width:170rpx;height:270rpx;"  @click="gotodetails(index)"></image>
				</view>
				<view style="width:400rpx;" class="center">
					<view style="font-size:36rpx;" class="name">
						{{item.name}}
					</view>
					<view style="padding-top:10rpx;">
						<uni-rate :value="item.score/2" :size="12" />
					</view>
					<view style="font-size:28rpx;">
						{{item.basicInfo}}
					</view>
					<view style="font-size:28rpx;">
						英雄
					</view>
					<view style="font-size:28rpx;" class="name">
						{{item.releaseDate}}
					</view>
				</view>
				<view style="border-left:4rpx dashed #eee;display:flex;align-items:center;flex-wrap:wrap;width:100rpx;">
					<view style="width:100%;text-align:center;" >
						<image src="../../static/Fabulous.png" mode="" v-if="flag " style="width:64rpx;height:64rpx" @click="Fabulous"></image>
						<image src="../../static/Fabulou.png" mode="" v-else style="width:64rpx;height:64rpx" @click="notFabulous"></image>
					</view>
					<view style="width:100%;text-align:center;">
						赞一下
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
				banners: [], //轮播图
				premierpeague: [], //热门英超
				notice: [], //热门预告
				guessULike: [] ,//猜我喜欢
				flag:false//点赞
			}
		},
		methods: {
			//跳转详情
			gotodetail(e){
				console.log(this.premierpeague[e]);
				let detail=JSON.stringify(this.premierpeague[e])
				uni.navigateTo({
				    url: `../detail/detail?detail=${detail}`
				});
			},
			gotodetails(e){
				console.log(this.guessULike[e]);
				let detail=JSON.stringify(this.guessULike[e])
				uni.navigateTo({
				    url: `../detail/detail?detail=${detail}`
				});
			},
			// play(event) {
			// 	this.videoCtx.play();
			// 	uni.showToast({
			// 		title: '开始播放',
			// 		icon: 'none'
			// 	});
			// },
			// pause(event) {
			// 	this.videoCtx.pause();
			// 	uni.showToast({
			// 		title: '暂停播放',
			// 		icon: 'none'
			// 	});
			// },
			Fabulous(){
				this.flag=false
			},
			notFabulous(){
				this.flag=true
			},
			//轮播图
			postBanners() {
				uni.request({
					url: `${this.$api}/index/carousel/list?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							this.banners = res.data.data
							console.log(this.banners, "轮播图");
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//热门超英
			postHot() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=40699904&type=superhero`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							this.premierpeague = res.data.data
							console.log(this.premierpeague, "英超");
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//热门预告
			postNotice() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=40699904&type=trailer`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							this.notice = res.data.data
							console.log(this.notice, "预告");
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//猜我喜欢
			postguessULike() {
				uni.request({
					url: `${this.$api}/index/guessULike?qq=40699904`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							this.guessULike = res.data.data
							console.log(this.guessULike, "猜我喜欢");
						}
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onLoad(options) {
			uni.showLoading({
				title: "加载中..."
			})
			// uni.showToast({
			// 	title:"首页"
			// }),
			this.postBanners()
			this.postHot()
			this.postNotice()
			this.postguessULike()

		},
		onNavigationBarButtonTap(e) {
			console.log(e);
		},
		// onNavigationBarSearchInputChanged(e){
		// 	console.log(e);
		// },
		// onNavigationBarSearchInputClicked(e){
		// 	console.log(e);
		// }
		onNavigationBarSearchInputConfirmed(e) {
			console.log(e);
		}
	}
</script>

<style>
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
	.center>view{
		padding-top: 20rpx;
	}
	/deep/.uni-scroll-view::-webkit-scrollbar {
	/* 隐藏滚动条，但依旧具备可以滚动的功能 */
	display: none
	}
</style>
