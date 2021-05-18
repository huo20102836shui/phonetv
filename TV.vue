<template>
	<view>
		<!-- 视频播放组件 -->
		<video autoplay="true" :src="src" @error="error"></video>
		<!--   开启自动播放    播放路径为src变量  播放出差时调用error函数  
		@fullscreenchange="fullscreenchange"
		  fullscreenchange视频全屏时调用fullscreenchange函数  -->
	
		<!-- 使用for循环输出节目列表 -->
		<view class="list">
			<view class="listview" v-for="(item,index) in tv_rtmp">
				<button v-on:click="check_tv(index)">
					<text>{{index + 1 + "\t"}}</text>
					<text>{{item.name}}</text>
				</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				src: "", //视频播放地址
				tv_rtmp: [],//节目列表
				index: "" //当前节目所在视频播放页的位置(下标)
			}
		},
		onLoad(option) {
			this.tv_rtmp = getApp().globalData.final_tv_rtmp//获取节目列表
			
			uni.getNetworkType({ //获取网络状态//提示用户断网
				success: function(res) {
					if (res.networkType == "none") { //没有网络或者网络不好
						uni.showToast({
							title: "网络走丢了呢",
							duration: 1000,
							icon: "none"
						});
					}
				}
			});
			uni.onNetworkStatusChange(function(res) { //网络发生变更时
				if (res.isConnected) {//res.isConnected为布尔值，表示当前网络状态
					uni.showToast({
						title: "网络回来啦",
						duration: 1000,
						icon: "none"
					});
				} else
					uni.showToast({
						title: "网络走丢了呢",
						duration: 1000,
						icon: "none"
					});
			});
			
			if (option.id < 1000) {//option.id是用户在首页点击的按钮的下标//小于1000其实是判断option.id是否是数字
				this.index = option.id
				this.src = this.tv_rtmp[this.index].path
				uni.setNavigationBarTitle({ //修改标题为当前节目
					title: this.tv_rtmp[this.index].name
				})
			}else{
				this.index = 0
				this.src = option.id
				uni.setNavigationBarTitle({ //修改标题//防止页面崩溃后标题异常//不一定有效
					title: option.id
				})
			}
		},
		methods: {
			error: function() {

			},
			fullscreenchange: function(event) {
				//智能切换屏幕方向
				if (event.detail.fullScreen) { //全屏
					plus.screen.lockOrientation("landscape") //自动感应切换屏幕横屏的方向
				} else { //全屏切换为小屏
					plus.screen.lockOrientation("portrait-primary") //设置屏幕竖直
				}
			},
			check_tv: function(index) {//切换电视频道
				this.src = this.tv_rtmp[index].path
				this.index = index
				uni.setNavigationBarTitle({ //修改标题//防止页面崩溃后标题异常//不一定有效
					title: this.tv_rtmp[this.index].name
				})
			},
		}
	}
</script>

<style>
	video {
		width: 750rpx;
		position: fixed;
		top: 0;
		left: 0;
	}

	.list {
		margin-top: 470rpx;
	}

	.listview {
		margin-top: 30rpx;
	}
</style>
