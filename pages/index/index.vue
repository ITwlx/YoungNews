<template>
	<view class="main">
		<view class="scroll-out">
			<scroll-view scroll-x="true" class="scroll-x">
				<view class="nav-item" :class="currentIndex == index ? 'active':''" v-for="(item,index) in newsList"
					@click="changeIndex(index,item.id)" :key="index">{{item.classname}}</view>
			</scroll-view>
		</view>
		<view class="content">
			<view class="content-item" v-for="(item,index) in newsInfo" :key="index">
				<newsBox :item="item" @click.native="goDetail(item)"></newsBox>
			</view>
		</view>
		<view class="nopic" v-if="!newsInfo.length">
			<image src="../../static/kiss.jpg" mode="widthFix"></image>
		</view>
		<view class="loading" v-if="newsInfo.length">
			<view v-if="bottomTextShow == 1">
				没有更多了...
			</view>
			<view v-if="bottomTextShow == 2">
				正在加载中...
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentIndex: 0,
				newsList:[],
				newsInfo:[],
				currentPage:1,
				bottomTextShow:0, // 0默认情况 1没有更多了 2正在加载中
			}
		},
		onLoad() {
			this.getNews()
			this.getNewsInfo()
		},
		onReachBottom() {
			if(this.bottomTextShow == 1){
				return;
			}
			this.bottomTextShow = 2
			this.currentPage++
			this.getNewsInfo()
		},
		methods: {
			// 点击导航
			changeIndex(index,id) {
				this.bottomTextShow = 0
				this.currentPage = 1
				this.newsInfo = []
				this.currentIndex = index
				this.getNewsInfo(id)
			},
			// 跳转到详情页面
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/newsDetail/newsDetail?classid=${item.classid}&id=${item.id}`
				})
			},
			// 获取新闻栏目
			async getNews(){
				await uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success:res=>{
						this.newsList = res.data
					}
				})
			},
			// 获取新闻栏目列表的具体内容
			async getNewsInfo(id = 50){
				await uni.request({
					url:'https://ku.qingnian8.com/dataApi/news/newslist.php',
					data:{
						cid:id,
						page:this.currentPage
					},
					success: (res) => {
						if(res.data.length==0){
							this.bottomTextShow = 1
						}
						
						this.newsInfo = [...this.newsInfo,...res.data]
					}
				})
			}

			}
		}
</script>

<style scoped lang="scss">
	.main {
		.scroll-out {}

		.scroll-x {
			position: fixed;
			top: var(--window-top);
			left: 0;
			z-index: 2;
			height: 100rpx;
			white-space: nowrap;
			background-color: #F7F8FA;

			::-webkit-scrollbar {
				display: none !important;
				width: 0 !important;
				height: 0 !important;
				-webkit-appearance: none;
				background: transparent;
			}

			.nav-item {
				line-height: 100rpx;
				display: inline-block;
				padding: 0 20px;
				color: #333;

				&.active {
					color: #31C27C;
				}
			}

		}

		.content {
			padding-top: 130rpx;
		}
		.loading{
			text-align: center;
			height: 50rpx;
			view{
				height: 50rpx;
				line-height: 50rpx;
			}
		}
	}
</style>
