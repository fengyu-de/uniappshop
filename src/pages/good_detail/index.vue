<template>
  <view>
    <!-- 轮播图 -->
    <swiper class="swiperbox" autoplay indicator-dots >
      <swiper-item v-for="(item,index) in goodinfo.pics" :key="item.pics_id"  @click="fun(index)">
        <image :src="item.pics_mid_url" mode="widthFix"></image>
      </swiper-item>
    </swiper>
    <!-- 价格名称 -->
    <view class="good_price">￥&nbsp;{{goodinfo.goods_price}}</view>
    <view class="goods_info">
      <view class="good_name">{{goodinfo.goods_name}}</view>
      <view class="good_start">
        <i class="iconfont icon-shoucang"></i>
        <view class="shoucang">收藏</view>
      </view>
    </view>
    <!-- 图文详情 -->
    <view class="goods_in">
      <view class="good_title">图文详情</view>
      <view class="good_content" v-html="goodinfo.goods_introduce"></view>
    </view>
    <!-- 工具栏 -->
    <view class="tool">
      <view class="tool_item">
        <view class="item">
          <i class="iconfont icon-kefu"></i>
          <view class="item_text">
            联系客服
            <button open-type="contact"></button>
          </view>
        </view>
        <view class="item">
           <i class="iconfont icon-fenxiang"></i>
          <view class="item_text">
            分享
            <button open-type="share"></button>
          </view>
        </view>
        <view class="item" @click="fn">
          <i class="iconfont icon-gouwuchekong"></i>
          <view class="item_text">购物车</view>
          </view>
      </view>
       <view class="tool_item">
         <view class="gocar" @click="func">加入购物车</view>
         <view class="pay">立即购买</view>
       </view>
    </view>
  </view> 
</template>

<script>
export default {
  data() {
    return {
      goods_id: 0, //请求参数
      goodinfo: {}
    };
  },
  onLoad(e) {
    console.log(e);
    this.goods_id = e.id;
  },
  mounted() {
    this.getlist(); //获取商品详情数据
  },
  methods: {
    async getlist() {
      let res = await uni.request({
        url: `https://api-hmugo-web.itheima.net/api/public/v1/goods/detail?goods_id=${this.goods_id}`,

      });
      this.goodinfo=res[1].data.message
      console.log(res[1].data.message);
    //   // 将商品id
    //   uni.setStorage({key:"cart",data:[goods_id=0]})
    },
    // 图片预览事件
    fun(index){
      console.log("点我了")
      let listimgurl=this.goodinfo.pics.map(i=>i.pics_mid_url)
      uni.previewImage({
        urls:listimgurl,  //预览的数据
        current:listimgurl[index], //从第几张开始预览
        loop:true  //循环预览
      })
    },
    // 跳转到购物车页面的方法
    fn(){
      // uni.switchTab是跳转到tabber页码(uni.navigateTo不能跳转到tabber页码)
      uni.switchTab({
        url:"/pages/cart/index"
      })
    },
    // 加入购物车事件
   async func(){
       // 1.获取缓存中的购物车 （数组）
        let cart=await uni.getStorage({key:"cart"})
      if(cart.length==1){
        cart=[]
      }else{
        cart=cart[1].data
      }
      console.log(cart)
      // 2.判断商品对象是否存在于购物车数组中
      let index=cart.findIndex((v)=>{
       return v.goods_id==this.goodinfo.goods_id
      })
      console.log(index)
      if(index==-1){
        // 3.不存在 第一次添加
      this.goodinfo.num=1  //数量
      this.goodinfo.checked=true  //默认选中
      cart.push(this.goodinfo)
      }else{
      //   // 4已经存在购物车数据 执行num++
        cart[index].num++
      }
    //   // // 把数据重新添加到缓存中
     await uni.setStorage({key:"cart",data:cart})
      // 弹框提示
      uni.showToast({
        title:"购物车加入成功"
      })
      // let cart2=await uni.getStorage({key:"cart"})
      // console.log(cart2)
    }
  }
};
// 加入购物车逻辑
// 1.绑定事件
// 2.获取缓存中的购物车数据(数组)
// 3.先判断当前的商品是否已经存在于缓存中
// 4.已经存在 修改商品数据 数量++重新保存到缓存在
// 5.不存在于购物车的数组中，则从新添加新数据
// 6.弹出提示
</script>
<style lang="scss">
.swiperbox{
  width: 100vw;
  height: 30vh;
  swiper-item{
    text-align: center;
    image{
      width: 50%;
    }
  }
}
// 商品详情
.good_price{
font-size: 30rpx;
color: #eb4450;
font-weight: 600;
}
.goods_info{
  display: flex;
  border-bottom: 2rpx solid #ccc;
  border-top: 2rpx solid #ccc;
  padding: 10rpx;
  .good_name{
    flex: 4;
    color: #000;
    font-size: 28rpx;
    padding-right: 20rpx;
    border-right: 2px solid #ccc;
    // 超过2行三个点表示
    display: -webkit-box;
    overflow: hidden;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
  }
  .good_start{
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-left: 20rpx;
    .icon-shoucang{

    }
    .shoucang{
      color: #000;
      font-size: 28rpx;
    }
  }
}
// 图文详情
.goods_in{
  .good_title{
    height: 50rpx;
    font-size: 35rpx;
    font-weight: 600;
    color: #eb4450;
    padding-left: 20rpx;
    border-bottom: 1px solid #ccc;
    .good_content{

    }
  }
}
// 小工具
.tool{
  width: 100%;
  display: flex;
  position: fixed;
  bottom: 0;
  left: 0;
  border-top: 1px solid #ccc;
.tool_item{
  flex: 1;
  display: flex;
  background-color: azure;
  .item{
    position: relative;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 10rpx;
    .item_text{
      color: #000;
      font-size: 18rpx;
      button{
        position: absolute;
        opacity: 0;
        left: 0;
        bottom: 0;
        width: 140rpx;
        height: 80rpx;
      }
    }
  }
}
.tool_item{
  flex: 1;
  display: flex;
  .gocar{
    flex: 1;
    display: flex;
    height: 100%;
    justify-content: center;
    color: #fff;
    font-size: 30rpx;
    align-items: center;
    background-color: orange;
  }
  .pay{
    flex: 1;
    height: 100%;
     display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    background-color: orangered;
  }
}
}
</style>
