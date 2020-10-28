<template>
	<!-- https://www.imovietrailer.com/superhero/doc.html -->
	<view>
		<view style="display:flex;justify-content:space-around;padding:20rpx;border-bottom:2rpx solid #eee;">
			<view style="display:flex;align-items:center;">
				<view class="seach">
					<image src="../../static/seachs.png" mode="" style="width:36rpx;height:36rpx;" @click="seachFilm"></image>
				</view>
				<view>
					<input class="uni-inputcss" focus placeholder="请输入搜索内容" @keydown.enter="seachFilm" v-model="keywords"/>
				</view>
				<view class="err">
					<image src="../../static/err.png" mode="" style="width:36rpx;height:36rpx;" @click="err"></image>
				</view>
			</view>
			<view style="padding:20rpx;color:#555;width:80rpx" @click="seachFilm">
				搜索	
			</view>
		</view>
		<view style="display:flex; justify-content:start;flex-wrap:wrap;">
			<view v-for="(item,index) in Allseach":key="item.id" style="width:240rpx;margin:4rpx">
				<image :src="item.cover" mode="" style="width:240rpx;height:300rpx;" @click="gotodetail(index)"></image>
				<view style="text-align:center;">
					{{item.name}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Allseach:[],//所有搜索
				keywords:'',//查询的名称
				page:1,//查询的下一个页面的页数，第[page]页
				pageSize:15//分页的每一页显示的条数
			}
		},
		methods: {
			//跳转详情
			gotodetail(e){
				console.log(this.Allseach[e]);
				let detail=JSON.stringify(this.Allseach[e])
				uni.navigateTo({
				    url: `../detail/detail?detail=${detail}`
				});
			},
			//热门预告
			postAllseach() {
				uni.request({
					url: `${this.$api}/search/list?qq=40699904&keywords=${this.keywords}&page=${this.page}&pageSize=${this.pageSize}`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							this.Allseach = res.data.data.rows
							console.log(this.Allseach, "全部搜索");
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//搜索
			seachFilm(){
				this.postAllseach() 
			},
			//清除搜索
			err(){
				this.keywords=''
				this.page=1
				this.pageSize=15
				this.postAllseach()
				
			}
		},
		//页面加载
		onLoad(options) {
			uni.showLoading({
				title: "加载中..."
			})
			this.postAllseach()
		},
		//上拉触底
		 onReachBottom(){
			  uni.showLoading({
			       title: '加载中...',
			     })
				this.page++
				uni.request({
					url: `${this.$api}/search/list?qq=40699904&keywords=${this.keywords}&page=${this.page}&pageSize=${this.pageSize}`,
					method: 'POST',
					data: {},
					success: res => {
						if (res.data.status === 200) {
							uni.hideLoading()
							console.log(res.data.data.rows.length,"上拉")
							if(res.data.data.rows.length>0){
								this.Allseach = this.Allseach.concat(res.data.data.rows)
							}else{
								uni.showToast({
										title: '暂无数据',
										icon: 'none'
									});
							}
							console.log(this.Allseach, "全部搜索");
						}
					},
					fail: () => {},
					complete: () => {}
				});
		 }
	}
</script>

<style>
	.uni-inputcss {
		border-top: 2rpx solid #eee;
		border-bottom: 2rpx solid #eee;
		padding: 10rpx;
		height:48rpx;
		line-height: 48rpx;
	}
	.seach{
		border-top: 2rpx solid #eee;
		border-bottom: 2rpx solid #eee;
		border-left: 2rpx solid #eee;
		border-top-left-radius: 50%;
		border-bottom-left-radius: 50%;
		padding: 10rpx;
		height:48rpx;
		display: flex;
		align-items:center;
	}
	.err{
		border-top: 2rpx solid #eee;
		border-bottom: 2rpx solid #eee;
		border-right: 2rpx solid #eee;
		border-top-right-radius: 50%;
		border-bottom-right-radius: 50%;
		padding: 10rpx;
		height:48rpx;
		display: flex;
		align-items:center;
	}
</style>
