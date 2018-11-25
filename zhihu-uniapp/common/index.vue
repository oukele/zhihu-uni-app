<template>

	<view style="height: auto;width: 100%;display: inline-block;">
		<view style="height: auto;width: 100%;">
			<swiper style="width: 100%;" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
				<swiper-item v-for="(item,index) in title_content" :key="index"  @tap="top_detail" :data-newsid="item.id">

					<view style='position: absolute;top: 70px;line-height: 25px;color: #F7F7F7;font-size: 17px;'>
						&emsp; {{item.title}}
					</view>
					<image :src="item.image" mode=""></image>

					<!-- <image src="https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1543036646&di=ac3c4c28c6f9e7af7d192471c3521cbe&src=http://img17.3lian.com/d/file/201703/10/af3c23bfb5c55952a1b535c83f293944.jpg" mode=""></image> -->
				</swiper-item>
			</swiper>

		</view>
		<!-- 列表内容区域 -->
		<view class="">今日热闻</view>
		<view class="content">
			<view class="uni-list">
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in list_content" :key="index"
				@tap="detail" :data-news_id="item.news_id">
					<view class="uni-media-list">
						<image class="uni-media-list-logo" :src="item.thumbnail"></image>
						<view class="uni-media-list-body">
							<view class="uni-media-list-text-top">{{item.title}}</view>
							<view class="uni-media-list-text-bottom uni-ellipsis">{{item.content}}</view>
						</view>
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
				list_content: [],
				title_content: []
			};
		},
		methods: {
			top_detail(e) {
				var id = e.currentTarget.dataset.newsid;
				uni.navigateTo({
					url: "../info/top?id=" + id
				})
			},
			detail(e) {
				uni.navigateTo({
					//https://news-at.zhihu.com/api/3/news/hot
					url: "../info/list?id=" + e.currentTarget.dataset.news_id
				})
			}
		},
		onLoad() {
			uni.showLoading({
				title: "加载中...."
			});

			uni.request({
				url: 'https://news-at.zhihu.com/api/4/news/latest',
				method: 'GET',
				data: {},
				success: res => {
					this.title_content = res.data.top_stories;
				},
				fail: () => {},
				complete: () => {}
			});

			uni.request({
				url: 'https://news-at.zhihu.com/api/3/news/hot',
				method: 'GET',
				data: {},
				success: res => {
					this.list_content = res.data.recent;
					uni.hideLoading();
				},
				fail: () => {},
				complete: () => {}
			})
		}
	}
</script>

<style>
	.content {
		height: auto;
	}
</style>
