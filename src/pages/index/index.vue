<template>
  <view>
	  <!-- 搜索 -->
    <serchInput></serchInput>
	<!-- 轮播 -->
	<swiper class="swiperbox" indicator-dots autoplay interval="3000" duration="1000" circular>
		<navigator :url="item.navigator_url">
		<swiper-item v-for="item in swiperlist" :key="item.goods_id">
			<image :src="item.image_src" mode="widthFix"></image>
		</swiper-item>
		</navigator>
	</swiper>
	<!-- 首页导航 -->
	<view class="navbox">
		<view class="nav_item" v-for="item in navlist" :key="item.name">
			<navigator :url="item.navigator_url">
			<image :src="item.image_src" mode="widthFix"></image>
			</navigator>
		</view>
	</view>
  </view>
</template>

<script>
import serchInput from "@/components/serchinput/index";
export default {
  data() {
    return {
	  swiperlist: [], //轮播图数据
	  navlist:[]      //首页导航数据
    };
  },
  onLoad() {},
  mounted() {
	this.getswiperlist(); //获取轮播图方法
	this.getnavlist()     //获取首页导航
  },
  methods: {
    async getswiperlist() {
      let res = await uni.request({
        url: "https://api-hmugo-web.itheima.net/api/public/v1/home/swiperdata"
	  });
	  this.swiperlist=res[1].data.message
      console.log(res[1].data.message);
	},
	async getnavlist(){
	let res=await	uni.request({
			url:"https://api-hmugo-web.itheima.net/api/public/v1/home/catitems"
		})
		this.navlist=res[1].data.message
		console.log(res[1].data.message)
	}
  },
  components: {
    serchInput
  }
};
</script>

<style lang="scss">
.swiperbox{
	width: 750rpx;
	height: 340rpx;
	padding: 5rpx;
		image{
			width: 100%;
		}
}
// 导航样式
.navbox{
	display: flex;
	padding: 10rpx;
	.nav_item{
		flex: 1;
		image{
			width: 100%;
		}
	}
}
</style>
