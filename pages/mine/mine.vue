<template>
	<view class="mine">
		<view class="mine-top">
			<view class="image">
				<image src="/static/clock.png" mode=""></image>
				<text>浏览历史</text>
			</view>
		</view>
		<view class="content">
			<view class="item" v-for="item in historyArr">
				<newsBox @click.native="goDetail(item)" :item="item"></newsBox>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyArr: []
			};
		},
		methods: {
			getData() {
				let arr = uni.getStorageSync('historyKey') || []
				this.historyArr = arr
				console.log(this.historyArr);
			},
			goDetail(item){
				uni.reLaunch({
					url:`/pages/newsDetail/newsDetail?cid=${item.classid}&id=${item.id}`
				})
			}
		},
		onShow() {
			this.getData()
		}
	}
</script>

<style lang="scss" scoped>
	.mine {
		.mine-top {
			height: 400rpx;
			background-color: #F7F7F7;

			.image {
				height: 400rpx;
				padding: 80rpx 0 50rpx 0;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				align-items: center;

				image {
					width: 200rpx;
					height: 200rpx;
				}
			}
		}
	}
</style>
