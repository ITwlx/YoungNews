<template>
	<view class="news-box">
		<view class="news-img">
			<image :src="item.picurl" mode=""></image>
		</view>
		<view class="news-content">
			<view class="title">
				{{item.title}}
			</view>
			<view class="auther">
				<view>{{item.author + '&nbsp;'+ '&nbsp;'+ '&nbsp;'}}</view>
				<view v-if="!item.visitTime">{{item.hits}}浏览</view>
				<view class="info" v-else>
					<text>浏览时间:{{item.visitTime}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "newsItem",
		props: {
			item: {
				type: Object,
				default () {
					return {
						title: '默认标题',
						author: '默认作者',
						hits: 668,
						picurl: "/static/loading.png",
						posttime: '2022-10-10'
					}
				}
			}
		},
		data() {
			return {

			};
		},
	}
</script>

<style lang="scss" scoped>
	.news-box {
		padding: 15rpx 0;
		display: flex;
		justify-content: space-between;

		.news-img {
			width: 230rpx;
			height: 160rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.news-content {
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			flex: 1;

			.title {
				font-size: 36rpx;
				text-overflow: -o-ellipsis-lastline;
				overflow: hidden; //溢出内容隐藏
				text-overflow: ellipsis; //文本溢出部分用省略号表示
				display: -webkit-box; //特别显示模式
				-webkit-line-clamp: 2; //行数
				line-clamp: 2;
				-webkit-box-orient: vertical; //盒子中内容竖直排列
			}

			.auther {
				font-size: 20rpx;
				color: #333;
			}

			.info {
				font-size: 22rpx;
			}
		}
	}
</style>
