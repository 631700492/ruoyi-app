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


		<uni-section title="本地数据" type="line">
			<u-row customStyle="margin-bottom: 10px">
				<u-col span="3">
					<u-list>
						<u-list-item v-for="(item, index) in indexList" :key="index">
							<u-cell :title="`列表长度-${index + 1}`">
								<u-avatar size="35" :src="item.url" customStyle="margin: -3px 5px -3px 0"></u-avatar>
							</u-cell>
						</u-list-item>
					</u-list>
				</u-col>
				<u-col span="9">
					<uni-section title="卡片封面图+操作栏" type="line">
						<uni-card :cover="cover" @click="onClick">
							<image slot='cover' style="width: 100%;" :src="cover"></image>
							<text class="uni-body">这是一个带封面和操作栏的卡片示例，此示例展示了封面插槽和操作栏插槽的用法。</text>
							
						</uni-card>
					</uni-section>
				</u-col>
			</u-row>

		</uni-section>

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
				cover:'static/images/banner/banner01.jpg',
				indexList: ['标题1', '标题1', '标题1', '标题1', '标题1']
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
