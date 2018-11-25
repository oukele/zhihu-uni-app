<template>
	<view class="content" style="display: inline-block;">
		<view class="top">
			{{title}}
		</view>
		<view class="body">
			<rich-text :nodes="msg"></rich-text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				msg: '',
				title: ''
			};
		},
		onLoad(e) {
			uni.showLoading({
				title: "加载中。。。。。"
			})
			uni.request({
				url: 'https://news-at.zhihu.com/api/4/news/' + e.id,
				method: 'GET',
				data: {},
				success: res => {
					this.title = res.data.title;
					this.msg = res.data.body;
					this.msg = res.data.body.replace("“text/javascript”","\"text/javascript\"");
		

					uni.hideLoading();
				},
				fail: () => {},
				complete: () => {}
			});
		}
	}
</script>

<style>
.content{width: 100%;fiex-wrap:wrap;}
.top{line-height: 2em;font-weight:700 ;font-size:38upx ;}
.body{width: 100%;height: auto;}
</style>
