<template>
	<mescroll-uni @init="mescrollInit" :up="upOption" :down="downOption" @down="downCallback" @up="upCallback"
		:bottom="100">
		<view class="container">
			<view>
				<swiper class="swiper" :indicator-dots='".swiper-pagination"==null?false:true'
					:autoplay='autoplaySwiper' :circular='true' indicator-active-color='#000000'
					indicator-color='rgba(0, 0, 0, .3)' :duration='1000' :interval='intervalSwiper'
					:vertical='"horizontal"=="horizontal"?false:true'>
					<swiper-item v-for="(swiper,index) in swiperList" :key="index">
						<image mode="aspectFill" :src="swiper"></image>
					</swiper-item>
				</swiper>
			</view>
			<view class="detail-content">
				<view class="price detail-list-item priceFavor"
					:style='{"boxShadow":"0 0 2rpx rgba(0,0,0,.8)","margin":"0 ","borderColor":"#fff","backgroundColor":"#fff","borderRadius":"0","borderWidth":"0","borderStyle":"solid","height":"68rpx"}'>
					<!-- 名字-->
					<view :style='{"textAlign":"left","fontSize":"45upx","color":"black"}'>{{detail.jiaoshiName}}
					</view>
				</view>
				<view style="margin-top: 10rpx;"></view>
				<view class="detail-list-item"
					:style='{"boxShadow":"0 0 4rpx 0 #006633 insta","margin":"0 60rpx 0 60rpx","borderColor":"rgba(218, 218, 218, 1)","backgroundColor":"#fff","borderRadius":"0","borderWidth":"2rpx","borderStyle":"none none solid none ","height":"68rpx"}'>
					<view class="lable"
						:style='{"width":"auto","fontSize":"28rpx","color":"#A27DE3","textAlign":"left"}'>
						教室类型：</view>
					<view class="text" :style='{"color":"#A27DE3","textAlign":"center","fontSize":"24rpx"}'>
						{{detail.jiaoshiValue}}
					</view>
				</view>
				<view class="detail-list-item"
					:style='{"boxShadow":"0 0 4rpx 0 #006633 insta","margin":"0 60rpx 0 60rpx","borderColor":"rgba(218, 218, 218, 1)","backgroundColor":"#fff","borderRadius":"0","borderWidth":"2rpx","borderStyle":"none none solid none ","height":"68rpx"}'>
					<view class="lable"
						:style='{"width":"auto","fontSize":"28rpx","color":"#A27DE3","textAlign":"left"}'>
						教室地点：</view>
					<view class="text" :style='{"color":"#A27DE3","textAlign":"center","fontSize":"24rpx"}'>
						{{detail.jiaoshiAddress}}
					</view>
				</view>
				<view class="detail-list-item"
					:style='{"boxShadow":"0 0 4rpx 0 #006633 insta","margin":"0 60rpx 0 60rpx","borderColor":"rgba(218, 218, 218, 1)","backgroundColor":"#fff","borderRadius":"0","borderWidth":"2rpx","borderStyle":"none none solid none ","height":"68rpx"}'>
					<view class="lable"
						:style='{"width":"auto","fontSize":"28rpx","color":"#A27DE3","textAlign":"left"}'>
						楼层：</view>
					<view class="text" :style='{"color":"#A27DE3","textAlign":"center","fontSize":"24rpx"}'>
						{{detail.loucengValue}}
					</view>
				</view>
				<view class="detail-list-item"
					:style='{"boxShadow":"0 0 4rpx 0 #006633 insta","margin":"0 60rpx 0 60rpx","borderColor":"rgba(218, 218, 218, 1)","backgroundColor":"#fff","borderRadius":"0","borderWidth":"2rpx","borderStyle":"none none solid none ","height":"68rpx"}'>
					<view class="lable"
						:style='{"width":"auto","fontSize":"28rpx","color":"#A27DE3","textAlign":"left"}'>
						教室状态：</view>
					<view class="text" :style='{"color":"#A27DE3","textAlign":"center","fontSize":"24rpx"}'>
						{{detail.jiaoshiOrderValue}}
					</view>
				</view>
			</view>
			<view class="time-content"
				:style='{"boxShadow":"0 0 2rpx rgba(0,0,0,.8)","margin":"0","borderColor":"#fff","backgroundColor":"#fff","borderRadius":"0","borderWidth":"0","borderStyle":"solid","height":"auto"}'>
				<view class="header" :style='{"color":"#333","fontSize":"28rpx"}'>
					简介/详情
				</view>
				<view class="content" style="line-height: 50upx;letter-spacing: 5upx;"
					:style='{"color":"#333","fontSize":"28rpx"}'>
					<rich-text :nodes="detail.jiaoshiContent"></rich-text>
				</view>
			</view>

			<view class="seat-list">
				<view style="margin-bottom: 20rpx;"
					:style='{"boxShadow":"0","backgroundColor":"rgba(255, 255, 255, 0.25)","borderColor":"rgba(228, 232, 230, 1)","margin":"0 0 0px 0","borderWidth":"2rpx","borderStyle":"none none solid none ","height":"88rpx"}'
					class="cu-form-group">
					<view :style='{"width":"160rpx","fontSize":"28rpx","color":"#A27DE3","textAlign":"left"}'
						class="title">
						订购日期</view>
					<input v-model="buyZuoweiTime" placeholder="订购日期" @tap="toggleTab('buyZuoweiTime')"></input>
					<!-- zuoweiChaxun -->
				</view>
				<view v-for="(item,index) in zuoweiNumberList " v-bind:key="index" class="seat-item">
					<image @click="$utils.msg('该座位已被预定')" v-if="item.select" class="seat-icon"
						src="../../static/restaurant-detail/select.png"></image>
					<image @click="selectSeat(item)" v-else-if="!item.active" class="seat-icon"
						src="../../static/restaurant-detail/unselect.png"></image>
					<image @click="unselectSeat(item)" v-else class="seat-icon"
						src="../../static/restaurant-detail/now.png"></image>
					<text>{{item.name}}</text>
				</view>
			</view>


			<view class="bottom-content cu-bar bg-white tabbar border shop">
				<view style="text-align: left;display: flex;">
				</view>
				<view style="text-align: right;">

					<button @tap="onSubmitTap" style="margin-right: 10upx;min-width: 180upx;"
						class="btn-submit cu-btn round bg-red" v-if="detail.jiaoshiOrderTypes == 2"
						:style="{width:'auto',borderRadius:'40rpx',height:'80rpx',fontSize:'28rpx',color:'rgba(239, 239, 239, 1)',backgroundColor:btnColor[2],borderColor:btnColor[2]}">预定座位</button>
					<button @tap="shenqingjiaoshi" style="margin-right: 10upx;min-width: 180upx;"
						class="btn-submit cu-btn round bg-red" v-if="detail.jiaoshiOrderTypes == 2 && jiaoshiOrde<= 0"
						:style="{width:'auto',borderRadius:'40rpx',height:'80rpx',fontSize:'28rpx',color:'rgba(239, 239, 239, 1)',backgroundColor:btnColor[2],borderColor:btnColor[2]}">预定教室</button>
				</view>
			</view>
			
		</view>
			
		<w-picker mode="date" step="1" :current="false" :hasSecond="false" @confirm="buyZuoweiTimeConfirm"
			ref="buyZuoweiTime" themeColor="#333333"></w-picker>
	</mescroll-uni>
</template>

<script>
	export default {
		data() {
			return {
				autoplaySwiper: false ? true : false,
				intervalSwiper: false ? $template2.front.base.swiper.autoplay.delay : 5000,
				btnColor: ['#409eff', '#67c23a', '#909399', '#e6a23c', '#f56c6c', '#356c6c', '#351c6c', '#f093a9',
					'#a7c23a', '#104eff', '#10441f', '#a21233', '#503319'
				],
				id: '',
				detail: {}, //本页面数据对象
				swiperList: [], //主图
				commentList: [], //评论留言数据集合
				mescroll: null, //mescroll实例对象
				downOption: {
					auto: false //是否在初始化后,自动执行下拉回调callback; 默认true
				},
				upOption: {
					noMoreSize: 3, //如果列表已无数据,可设置列表的总数量要大于半页才显示无更多数据;避免列表数据过少(比如只有一条数据),显示无更多数据会不好看; 默认5
					textNoMore: '~ 没有更多了 ~',
				},
				hasNext: true,
				user: {}, //登录用户信息
				collectionFlag: 0, //收藏
				zuoweiNumberList: [], //订座
				jiaoshiOrde:null,
				buyZuoweiTime: this.$utils.getCurDate(), //订座时间
			}
		},
		computed: {
			baseUrl() {
				return this.$base.url;
			},
		},
		async onLoad(options) {
			this.id = options.id;
		},
		async onShow(options) {
			// 渲染数据
			this.init();
			let table = uni.getStorageSync("nowTable");
			// 获取用户信息
			let res = await this.$api.session(table);
			this.user = res.data;
			this.btnColor = this.btnColor.sort(() => {
				return (0.5 - Math.random());
			});
			let cleanType = uni.getStorageSync('pingluenStateState')
			if (cleanType) {
				uni.removeStorageSync('pingluenStateState')
				this.mescroll.num = 1
				this.upCallback(this.mescroll)
			}
		},
		destroyed: function() {
			//window.clearInterval(this.inter);
		},
		methods: {
			shenqingjiaoshi(){
				this.$utils.jump('../jiaoshishenqing/add-or-update?jiaoshiId='+this.detail.id);
			},
			//下载
			download(url) {
				let _this = this;
				uni.downloadFile({
					url: url,
					success: (res) => {
						if (res.statusCode === 200) {
							_this.$utils.msg('下载成功');
							window.open(url);
						}
					}
				});
			},
			// 获取详情
			async init() {
				let res = await this.$api.info('jiaoshi', this.id);
				this.detail = res.data;
				// 主图
				this.swiperList = this.detail.jiaoshiPhoto ? this.detail.jiaoshiPhoto.split(",") : [];

				this.zuoweiChange();

			},
			async zuoweiChange() {
				//订座
				let _this = this
				let res = await _this.$api.list('jiaoshiOrder', {
					page: 1,
					limit: 100,
					jiaoshiId: _this.detail.id,
					jiaoshiOrderDelete: 1,
					jiaoshiOrderTypes: 1,
					buyZuoweiTimeStart: _this.buyZuoweiTime,
					buyZuoweiTimeEnd: _this.buyZuoweiTime,
				})
				_this.jiaoshiOrde = res.data.total
				//订座
				_this.zuoweiNumberList = [];
				for (let i = 1; i <= _this.detail.zuoweiNumber; i++) {
					_this.zuoweiNumberList.push({
						name: `${i}号`,
						select: false,
						active: false
					});
				}
				if (res.data != null && res.data != "" && res.data.list.length > 0) {
					res.data.list.forEach(element => {
						if (element.buyZuoweiNumber != null) {
							for (let i = 0; i < element.buyZuoweiNumber.split(',').length; i++) {
								_this.zuoweiNumberList[element.buyZuoweiNumber.split(',')[i] - 1].select =
								true;
							}
						}
					});
				}
			},
			// 选座座位
			selectSeat(item) {
				item.active = true;
			},
			// 取消选择座位
			unselectSeat(item) {
				item.active = false;
			},
			async onSubmitTap() {
				let _this = this;
				uni.showModal({
					title: '提示',
					content: '是否确认预定',
					success: async function(res) {
						if (res.confirm) {
							let activeZuoweiList = [];
							for (let i = 0; i < _this.zuoweiNumberList.length; i++) {
								if (_this.zuoweiNumberList[i].active) {
									activeZuoweiList.push(_this.zuoweiNumberList[i].name.replace('号', ''));
								}
							}
							if (activeZuoweiList.length == 0) {
								_this.$utils.msg('请选择要预定的座位');
								return false;
							}
							let data = [{
								jiaoshiId: _this.detail.id,
								selected: activeZuoweiList.toString(),
								jiaoshiName: _this.detail.jiaoshiName,
								buyZuoweiTime: _this.buyZuoweiTime,
								jiaoshiPhoto: _this.swiperList[0],
							}]
							uni.setStorageSync('orderGoods', data);
							_this.$utils.jump('../jiaoshiOrder/confirm');
						}
					}
				});
			},
			// mescroll组件初始化的回调,可获取到mescroll对象
			mescrollInit(mescroll) {
				this.mescroll = mescroll;
			},

			/*下拉刷新的回调 */
			downCallback(mescroll) {
				this.hasNext = true
				mescroll.resetUpScroll()
			},

			/*上拉加载的回调: mescroll携带page的参数, 其中num:当前页 从1开始, size:每页数据条数,默认10 */
			async upCallback(mescroll) {},
			// 日期控件
			buyZuoweiTimeConfirm(val) {
				this.buyZuoweiTime = val.result;
				this.zuoweiChange();
				this.$forceUpdate();
			},
			toggleTab(str) {
				this.$refs[str].show();
			}
		}
	}
</script>

<style lang="scss">
	.container {
		// padding-bottom: 80upx;
	}

	.container:after {
		position: fixed;
		top: 0;
		right: 0;
		left: 0;
		bottom: 0;
		content: '';
		background-attachment: fixed;
		background-size: cover;
		background-position: center;
	}

	.swiper {
		width: 100%;
		height: 450upx;

		image {
			width: 100%;
		}
	}

	.detail-content {
		font-size: 28upx;
		color: #888888;
		// padding: 10upx 20upx;
		background: #FFFFFF;
		line-height: 45upx;

		.name {
			font-size: 38rpx;
			font-weight: blod;
			// margin-bottom: 20upx;
			padding-top: 20upx;
		}

		.price {
			font-size: 33rpx;
		}
	}

	.time-content {
		background: #FFFFFF;
		padding: 30upx;
		font-size: 30upx;
		border-radius: 20upx;
		margin-top: 20upx;

		.header {
			font-size: 33upx;
			margin-bottom: 20upx;
		}
	}

	.bottom-content {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
	}

	.comoment-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-bottom: 12upx;

		.btn-comment-content {
			color: #DD514C;
			font-weight: bold;
			padding: 0 12upx;
		}
	}

	.comment-item {
		margin-bottom: 10upx;
		border-bottom: 1px solid #EEEEEE;
		padding-bottom: 20upx;

		.content {
			margin-left: 20upx;
			margin-top: 20upx;
		}
	}

	.icon {
		width: 50upx;
		height: 50upx;
	}

	.popup-content {
		.popup-form {
			background: #FFFFFF;
			margin-top: 20upx;
		}

		.btn-content {
			text-align: right;
			background: #FFFFFF;

			button {
				margin-left: 20upx;
				margin-right: 20upx;
				margin-bottom: 20upx;
			}
		}
	}

	.detail-list-item {
		padding: 0 24upx;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		height: 68upx;
		border-bottom: 2upx solid #efefef;
		background-color: #fff;

		.lable {
			font-size: 28upx;
			color: #000;
		}

		.text {
			flex: 1;
			font-size: 24upx;
			color: #FF00FF;
			text-align: left;
		}

		&.favor {
			.el-icon-star-on {
				color: inherit;
				font-size: inherit;
			}
		}
	}

	.detail-content .shop-title {
		padding: 20upx 24upx;
		box-sizing: border-box;
		line-height: 1.4;
	}

	.priceFavor {
		height: 96upx !important;
	}

	.priceFavor .text {
		font-size: 64upx !important;
		color: red !important;
	}

	.priceFavor .icon {
		font-size: 56upx !important;
		text-align: right !important;
		color: red !important;
	}

	//订座样式
	.seat-list {
		display: flex;
		align-items: center;
		flex-wrap: wrap;
		background: #FFFFFF;
		margin: 20upx;
		border-radius: 20upx;
		padding: 20upx;
		font-size: 30upx;

		.seat-item {
			width: 33.33%;
			display: flex;
			align-items: center;
			flex-direction: column;
			margin-bottom: 20upx;

			.seat-icon {
				width: 50upx;
				height: 50upx;
				margin-bottom: 10upx;
			}
		}
	}
</style>
