<template>
	<view class="work-container">
		<!-- 轮播图 -->
		<uni-swiper-dot class="uni-swiper-dot-box" :info="data" :current="current" field="content">
			<swiper class="swiper-box" :current="swiperDotIndex" @change="changeSwiper">
				<swiper-item v-for="(item, index) in data" :key="index">
					<view class="swiper-item" @click="clickBannerItem(item)">
						<image :src="imageUrl +item.images" mode="aspectFill" :draggable="false" />
					</view>
				</swiper-item>
			</swiper>
		</uni-swiper-dot>

		<view class="u-demo-block">
			<text class="u-demo-block__title">基础使用</text>
			<view class="u-demo-block__content">
				<u-row customStyle="margin-bottom: 10px">
					<u-col span="6">
						<view class="demo-layout bg-purple-light"></view>
					</u-col>
					<u-col span="6">
						<view class="demo-layout bg-purple"></view>
					</u-col>
				</u-row>
				<u-row customStyle="margin-bottom: 10px">
					<u-col span="4">
						<view class="demo-layout bg-purple"></view>
					</u-col>
					<u-col span="4">
						<view class="demo-layout bg-purple-light"></view>
					</u-col>
					<u-col span="4">
						<view class="demo-layout bg-purple-dark"></view>
					</u-col>
				</u-row>
				<u-row justify="space-between">
					<u-col span="3">
						<view class="demo-layout bg-purple">5</view>
					</u-col>
					<u-col span="3">
						<view class="demo-layout bg-purple-light">4</view>
					</u-col>
					<u-col span="3">
						<view class="demo-layout bg-purple">3</view>
					</u-col>
					<u-col span="3">
						<view class="demo-layout bg-purple-light">2</view>
					</u-col>
				</u-row>
			</view>
		</view>
	</view>

</template>

<script>
	import request from '@/utils/request'
	export default {
		data() {
			return {
				imageUrl: getApp().globalData.config.baseUrl,
				current: 0,
				swiperDotIndex: 0,
				data: [],
				value1: '1',
				value2: '2',
				show: true,
				options1: [{
						label: '默认排序',
						value: 1,
					},
					{
						label: '距离优先',
						value: 2,
					},
					{
						label: '价格优先',
						value: 3,
					}
				],
				options2: [{
						label: '去冰',
						value: 1,
					},
					{
						label: '加冰',
						value: 2,
					},
				]
			}
		},
		created() {
			this.getLunboTu();
		},
		onPullDownRefresh() {
			console.log('refresh');
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},

		methods: {
			getLunboTu() {
				var that = this
				request({
					url: '/weixin/getLunBoImage',
					method: 'get'
				}).then(function(data) {
					that.data = data.data
				})
			},
			clickBannerItem(item) {
				console.info(item)
			},
			changeSwiper(e) {
				this.current = e.detail.current
			},
			changeGrid(e) {
				this.$modal.showToast('模块建设中~')
			}
		}
	}
</script>

<style lang="scss">
	/* #ifndef APP-NVUE */
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color: #fff;
		min-height: 100%;
		height: auto;
	}

	view {
		font-size: 14px;
		line-height: inherit;
	}

	/* #endif */

	.text {
		text-align: center;
		font-size: 26rpx;
		margin-top: 10rpx;
	}

	.grid-item-box {
		flex: 1;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}

	.uni-margin-wrap {
		width: 690rpx;
		width: 100%;
		;
	}

	.swiper {
		height: 300rpx;
	}

	.swiper-box {
		height: 150px;
	}

	.swiper-item {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		justify-content: center;
		align-items: center;
		color: #fff;
		height: 300rpx;
		line-height: 300rpx;
	}

	@media screen and (min-width: 500px) {
		.uni-swiper-dot-box {
			width: 400px;
			/* #ifndef APP-NVUE */
			margin: 0 auto;
			/* #endif */
			margin-top: 8px;
		}

		.image {
			width: 100%;
		}
	}
</style>
