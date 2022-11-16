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



		<u-row>
			<!-- <u-list>
					<u-list-item v-for="(item, index) in indexList" :key="index">
						<u-cell :title="`列表长度-${index + 1}`">
							<u-avatar size="35" :src="item.url" customStyle="margin: -3px 5px -3px 0"></u-avatar>
						</u-cell>
					</u-list-item>
				</u-list> -->
			<u-tabs :list="list2" style="margin-top: 20px;font-size:24px!important;"  @click="checkCarType" :activeStyle="{
			            color: '#00aaff',
			            fontWeight: 'bold',
			            transform: 'scale(1.05)'
			        }" :inactiveStyle="{
			            color: '#606266',
			            transform: 'scale(1)'
			        }" itemStyle="padding-left: 15px; padding-right: 15px; height: 34px;"></u-tabs>
		</u-row>

		<view class="u-page">
			<u-list @scrolltolower='scrolltolower' :enableBackToTop="true">
				<u-list-item v-for="(item, index) in carList" :key="index">

					<uni-card :is-shadow="false">

						<u-row>
							<u-col :span="4">
								<u--image :src="imageUrl +item.images" :fade="true" duration="450" 
								:width='170'
								:height='170'></u--image>
							</u-col>
							<u-col :span="8">
								<text>{{item.carName}}</text>
								<text>{{item.carPrice}}</text>
								<text>{{item.carMemo}}</text>
							</u-col>
						</u-row>
		
					</uni-card>

				</u-list-item>
			</u-list>

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
				carList: [],
				carType: "0",
				cover: 'static/images/banner/banner01.jpg',
				list2: [{
					name: '所有类型'
				}, {
					name: '经济型'
				}, {
					name: '商务型'
				}, {
					name: 'SUV'
				}, {
					name: '豪华型'
				}]
			}
		},
		created() {
			this.getLunboTu();
			this.scrolltolower()
		},
		onPullDownRefresh() {
			console.log('refresh');
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},

		/* onLoad() {
			this.scrolltolower()
		}, */
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

			checkCarType(item) {
				console.log('item' + item)

				var carType = null
				this.carType = item.index
				if (item == null || item.index == 0) {
					carType = null
				} else {
					carType = item.index
				}
				var data = {
					"carType": carType,
					"pageNum": 1,
					"pageSize": 10
				}
				this.selectCarList(data)

			},
			selectCarList(data) {
				var that = this
				request({
					url: '/weixin/centalCarList',
					method: 'get',
					data: data
				}).then(function(data) {
					that.carList = data.data.rows
				})
			},
			scrolltolower() {

				var carType = null

				if (this.carType == null || this.carType == 0) {
					carType = null
				} else {
					carType = this.carType
				}
				var data = {
					"carType": carType,
					"pageNum": 1,
					"pageSize": 10
				}
				this.selectCarList(data)
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
