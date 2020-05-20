<template>
  <view>
    <!-- 收货地址 -->
    <view class="userinfo">
      <view class="username">
        <view class="name">收件人：{{userinfo.userName}}</view>
        <view class="tell">{{userinfo.telNumber}}</view>
      </view>
      <view
        class="city"
      >{{userinfo.provinceName}}{{userinfo.cityName}}{{userinfo.countyName}}{{userinfo.detailInfo}}</view>
    </view>
    <!-- 购物车标题 -->
    <view class="title">购物车</view>
    <!-- 商品列表 -->
    <view class="goods_List">
      <view class="warp">
        <view class="item" v-for="(item,index) in datalist" :key="index">
          <view class="img">
          <image mode="widthFix" :src="item.goods_small_logo"></image>
            </view>
            <view class="info">
              <view class="name">{{item.goods_name}}</view>
              <view class="price_num">
                <view class="price">￥{{item.goods_price}}</view>
                <view class="numm">x{{item.num}}</view>
              </view>
            </view>
        </view>
      </view>
    </view>
    <!-- 小工具 -->
    <view class="tool">
      <view class="price">
        <view class="num">
          <text>合计:&nbsp;&nbsp;￥&nbsp;&nbsp;666</text>
        </view>
        <text>包含运费</text>
      </view>
      <view class="pay">
        支付({{datalist.length}})
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      userinfo: {}, //用户数据
      datalist: [] //商品列表数据
    };
  },
  mounted() {
    this.getlist(); // 获取缓存中的数据
  },
  methods: {
    async getlist() {
      let res2 = await uni.getStorage({ key: "userzhu" });
      let res = await uni.getStorage({ key: "cart" });
      this.datalist = res[1].data;
      this.userinfo = res2[1].data[1];
      console.log(res)
      console.log(res2);
    }
  }
};
// 微信支付
</script>

<style lang="scss" scoped>
.userinfo {
  padding: 10rpx;
  .username {
    font-size: 25rpx;
    display: flex;
    justify-content: space-between;
    color: #000;
    .name {
      flex: 1;
    }
    .tell {
      flex: 1;
      text-align: right;
    }
  }
  .city {
    font-weight: 600;
    font-size: 30rpx;
  }
}
.title {
  font-size: 40rpx;
  color: orangered;
  padding: 15rpx;
  border-bottom: 2rpx solid orange;
  border-top: 2rpx solid orange;
}
// 商品列表
.goods_List {

  .warp {
    .item {
      display: flex;
      padding: 10rpx;
      border-bottom: 2rpx solid palevioletred;;
      .img {
        flex: 1;
        image {
          width: 80%;

        }
      }
        .info {
          flex: 2;
          .name {
            display: -webkit-box;
        overflow: hidden;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 1;
        color: #666;
          }

          .price_num {
            display: flex;
            padding: 10rpx;
            justify-content: space-between;
             margin-top: 100rpx;
            .price {
              color: orange;
              font-size: 25rpx;
            }

            .numm {
              color: #666;
              font-size: 22rpx;
            }
          }
        }
      
    }
  }
}
// 小工具
.tool{
  display: flex;
  justify-content: flex-end;
  .price{
    .num{
      text{
        font-size: 35rpx;
        color: orangered;
      }
    }
    text{
      font-size: 20rpx;

    }
  }
  .pay{
    color: #fff;
    font-size: 35rpx;
    text-align: center;
    line-height: 80rpx;
    width: 150rpx;
    height: 80rpx;
    background-color: orangered;
  }
}
</style>
