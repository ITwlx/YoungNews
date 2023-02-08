<template>
	<view>
		<view class="top">
			<view class="top-title">
				{{detail.title}}
			</view>
			<view class="top-subtitle">
				<view class="top-author">
					编辑：{{detail.author}}
				</view>
				<view class="top-date">
					{{detail.hits}}
				</view>
			</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>

		</view>
		<view class="description">
			声明:本站的内容均采集与腾讯新闻,如果侵权请联系管理(1921593177@qq.com)进行整改删除，本站进行了内容采集不代表本
			站及作者观点,著有侵犯清及时联系管理员，谢谢您的支持。
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				item: null,
				detail: {},
			};
		},
		methods: {
			getNewsInfo() {
				console.log(this.item);
				uni.request({
					url: 'https://ku.qingnian8.com/dataApi/news/detail.php',
					data: this.item,
					success: res => {
						res.data.content = res.data.content.replace(/<img/gi, "<img style='max-width:100%'")
						this.detail = res.data
						this.saveHistory()
						uni.setNavigationBarTitle({
							title: this.detail.title
						})
					}
				})
			},
			saveHistory() {
				let historyArr = uni.getStorageSync('historyKey') || []
				
				let {
					classid,
					id,
					picurl,
					title,
					hits,
				} = this.detail
				let index = historyArr.findIndex((i) => {
					return i.id == this.detail.id
				})
				if (index == 0) {
					historyArr = historyArr.slice(0,9)
					return
				} else {
					historyArr = historyArr.slice(0,9)
					historyArr.unshift({
						classid,
						id,
						picurl,
						title,
						hits
					})
					
					uni.setStorageSync('historyKey', historyArr)
				}

			}
		},
		onLoad(e) {
			this.item = e
			this.getNewsInfo()
		},
	}
</script>

<style lang="scss" scoped>
	.top {
		.top-title {
			height: 300rpx;
			font-size: 38rpx;
		}

		.top-subtitle {
			height: 50rpx;
			line-height: 30rpx;
			font-size: 24rpx;
			display: flex;
			justify-content: space-between;
			padding: 10rpx 30rpx;
			background-color: #F6F6F6;
		}
	}

	.content {
		padding: 20rpx;
	}

	.description {
		color: red;
		background-color: #FDEFF0;
		padding: 20rpx;
	}
</style>
