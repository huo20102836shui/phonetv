<template>
	<view class="content">
		
		<!-- 用for循环将每个节目的按钮及文字输出 -->
		<view class="listview" v-for="(item,index) in tv_rtmp">
			<button v-on:click="to_tv(index)">
				<text>{{index + 1 + "\t"}}</text>
				<text>{{item.name}}</text>
			</button>
		</view>
		
		<!-- 自助播放，点击后需要输入地址 -->
		<view class="other">
			<button v-on:click="play_other">自助播放</button>
		</view>
		
		<!-- 自定义的含输入框的弹出框 -->
		<prompt ref="prompt" @onConfirm="onConfirm" @onCancel="onCancel" title="播放地址" text="请输入播放链接"></prompt>
	</view>
</template>

<script>
	import prompt from '../../static/prompt.vue'; //导入自定义带输入框的弹出框
	export default {
		data() {
			return {
				//电视节目的取流地址及其电视台
				tv_rtmp: [
					{"name":"CCTV-1综合","path":"rtmp://58.200.131.2:1935/livetv/cctv1"},
					{"name":"CCTV-2财经","path":"rtmp://58.200.131.2:1935/livetv/cctv2"},
					{"name":"CCTV-3综艺","path":"rtmp://58.200.131.2:1935/livetv/cctv3"},
					{"name":"CCTV-4中文国际","path":"rtmp://58.200.131.2:1935/livetv/cctv4"},
					{"name":"CCTV-5体育","path":"rtmp://58.200.131.2:1935/livetv/cctv5"},
					{"name":"CCTV-6电影","path":"rtmp://58.200.131.2:1935/livetv/cctv6"},
					{"name":"CCTV-7军事农业","path":"rtmp://58.200.131.2:1935/livetv/cctv7"},
					{"name":"CCTV-8电视剧","path":"rtmp://58.200.131.2:1935/livetv/cctv8"},
					{"name":"CCTV-9记录","path":"rtmp://58.200.131.2:1935/livetv/cctv9"},
					{"name":"CCTV-10科教","path":"rtmp://58.200.131.2:1935/livetv/cctv10"},
					{"name":"CCTV-11戏曲","path":"rtmp://58.200.131.2:1935/livetv/cctv11"},
					{"name":"CCTV-12社会与法","path":"rtmp://58.200.131.2:1935/livetv/cctv12"},
					{"name":"CCTV-13新闻","path":"rtmp://58.200.131.2:1935/livetv/cctv13"},
					{"name":"CCTV-14少儿","path":"rtmp://58.200.131.2:1935/livetv/cctv14"},
					{"name":"CCTV-15音乐","path":"rtmp://58.200.131.2:1935/livetv/cctv15"},
					
					//电视频道末尾增加hd表示高清源
					
					{"name":"安徽卫视","path":"rtmp://58.200.131.2:1935/livetv/ahtv"},
					{"name":"兵团卫视","path":"rtmp://58.200.131.2:1935/livetv/bttv"},
					{"name":"重庆卫视","path":"rtmp://58.200.131.2:1935/livetv/cqtv"},
					{"name":"东方卫视","path":"rtmp://58.200.131.2:1935/livetv/dftv"},
					{"name":"东南卫视","path":"rtmp://58.200.131.2:1935/livetv/dntv"},
					{"name":"广东卫视","path":"rtmp://58.200.131.2:1935/livetv/gdtv"},
					{"name":"广西卫视","path":"rtmp://58.200.131.2:1935/livetv/gxtv"},
					{"name":"甘肃卫视","path":"rtmp://58.200.131.2:1935/livetv/gstv"},
					{"name":"贵州卫视","path":"rtmp://58.200.131.2:1935/livetv/gztv"},
					{"name":"湖北卫视","path":"rtmp://58.200.131.2:1935/livetv/hbtv"},
					{"name":"湖南卫视","path":"rtmp://58.200.131.2:1935/livetv/hunantv"},
					{"name":"河北卫视","path":"rtmp://58.200.131.2:1935/livetv/hebtv"},
					{"name":"河南卫视","path":"rtmp://58.200.131.2:1935/livetv/hntv"},
					{"name":"黑龙江卫视","path":"rtmp://58.200.131.2:1935/livetv/hljtv"},
					{"name":"江苏卫视","path":"rtmp://58.200.131.2:1935/livetv/jstv"},
					{"name":"江西卫视","path":"rtmp://58.200.131.2:1935/livetv/jxtv"},
					{"name":"吉林卫视","path":"rtmp://58.200.131.2:1935/livetv/jltv"},
					{"name":"辽宁卫视","path":"rtmp://58.200.131.2:1935/livetv/lntv"},
					{"name":"内蒙古卫视","path":"rtmp://58.200.131.2:1935/livetv/nmtv"},
					{"name":"宁夏卫视","path":"rtmp://58.200.131.2:1935/livetv/nxtv"},
					{"name":"青海卫视","path":"rtmp://58.200.131.2:1935/livetv/qhtv"},
					{"name":"四川卫视","path":"rtmp://58.200.131.2:1935/livetv/sctv"},
					{"name":"山东卫视","path":"rtmp://58.200.131.2:1935/livetv/sdtv"},
					{"name":"山西卫视","path":"rtmp://58.200.131.2:1935/livetv/sxrtv"},
					{"name":"陕西卫视","path":"rtmp://58.200.131.2:1935/livetv/sxtv"},
					{"name":"山东教育","path":"rtmp://58.200.131.2:1935/livetv/sdetv"},
					{"name":"中国教育1","path":"rtmp://58.200.131.2:1935/livetv/cetv1"},
					{"name":"中国教育3","path":"rtmp://58.200.131.2:1935/livetv/cetv3"},
					{"name":"中国教育4","path":"rtmp://58.200.131.2:1935/livetv/cetv4"},
					{"name":"中国教育2","path":"rtmp://58.200.131.2:1935/livetv/cetv2"},
					{"name":"CCTV-第一剧场","path":"rtmp://58.200.131.2:1935/livetv/dyjctv"},
					{"name":"CCTV-国防军事","path":"rtmp://58.200.131.2:1935/livetv/gfjstv"},
					{"name":"CCTV-怀旧剧场","path":"rtmp://58.200.131.2:1935/livetv/hjjctv"},
					{"name":"CCTV-风云剧场","path":"rtmp://58.200.131.2:1935/livetv/fyjctv"},
					{"name":"CCTV-风云足球","path":"rtmp://58.200.131.2:1935/livetv/fyzqtv"},
					{"name":"CCTV-风云音乐","path":"rtmp://58.200.131.2:1935/livetv/fyyytv"},
					{"name":"CCTV-世界地理","path":"rtmp://58.200.131.2:1935/livetv/sjdltv"},
					{"name":"CGTN-新闻","path":"rtmp://58.200.131.2:1935/livetv/cctv16"},
					//https://player.ibodhi.cn/donglin/hxkt_ld.m3u8
				]
			}
		},
		onLoad() {
			//将地址保存为全局变量，以便视频播放界面使用
			getApp().globalData.final_tv_rtmp = this.tv_rtmp
		},
		components: {//声明自定义组件
			prompt,
		},
		methods: {
			to_tv(index){//按钮的点击事件
				uni.navigateTo({//在当前页面基础上打开页面
					url: '/pages/TV/TV?id=' + index
				});
			},
			play_other(){
				this.prompt()
			},
			
			//自定义弹出输入框方法
			prompt: function() { //显示弹出框
					uni.pageScrollTo({ //返回顶部
						scrollTop: 0,
						duration: 0 //动画时长
					})
					this.$refs.prompt.show();
			},
			onConfirm: function(e) { //确定
				let _cost = e;
				if (_cost == '') { //空
					uni.showToast({
						icon:"none",
						duration:1500,
						title:'你还未输入'
					})
					return;
				} else { //输入了
					this.$refs.prompt.hide();
					uni.navigateTo({
						url: '/pages/TV/TV?id=' + _cost
					});
				}
			},
			onCancel: function() { //点击取消密码
				this.$refs.prompt.hide();
				this.$refs.prompt.cost = '';
			},
		}
	}
</script>

<style>
.listview{
	margin-top: 30rpx;
}
.other{
	margin-top: 60rpx;
}
</style>
