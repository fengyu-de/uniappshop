<template>
  <view>
    <!-- 导航 -->
    <Tabs :tabs="tabs" @func="fn"></Tabs>
    <!-- 分类 -->
    <view class="fenlei" v-if="num==1">
      <view :class="['item1',{'border':index==0}]" @click="fun(0)">全部</view>
      <view :class="['item',{'border':index==1}]" @click="fun(1)">正在热卖</view>
      <view :class="['item',{'border':index==2}]" @click="fun(2)">即将上线</view>
     </view>
    <!-- 商品列表 -->
    <view class="goodsbox"  v-if="num==1">
      <view class="gooditem" v-for="(item,index) in goodslist" :key="index">
        <view class="img">
          <image :src="item.goods_small_logo" mode="widthFix"></image>
        </view>
      <view class="goodinfo">
        <view class="title">{{item.goods_name}}</view>
        <view class="price">￥{{item.goods_price}}</view>
      </view>
      </view>
    </view>
   <view v-else-if="num==2">
      品牌收藏
   </view>
   <view v-else-if="num==3">
      店铺收藏
   </view>
    <view v-else>
      浏览足迹
   </view>
  </view>
</template>

<script>
import Tabs from "@/components/tabs/index"; //tabs切换
export default {
  components: {
    Tabs
  },
  data() {
    return {
      tabs: [
        {
          id: 0,
          title: "商品收藏",
          isActive: true
        },
        {
          id: 1,
          title: "品牌收藏",
          isActive: false
        },
        {
          id: 2,
          title: "店铺收藏",
          isActive: false
        },
        {
          id: 3,
          title: "浏览足迹",
          isActive: false
        }
      ],
      index: 0 ,  //选中分类的索引
      goodslist:[], //缓存中的数据
      num:0
    };
  },
  mounted() {
    this.getlist();
  },
  onLoad(e){
    console.log(e.num) //获取上一个页面传递过来的参数
    this.tabs.forEach(i=>i.isActive=false)   //将tabs的isActive改为false
    if(e.num==1){     //通过传递过来的参数来tabs的isActive是否为true
      this.tabs[0].isActive=true
    }else if(e.num==2){
      this.tabs[1].isActive=true
    }else if(e.num==3){
      this.tabs[2].isActive=true
    }else {
      this.tabs[3].isActive=true
    }
    this.num=e.num
  },
  methods: {
    // 头部导航切换事件
    fn(index) {
      this.tabs.forEach(i => {  //将tabs的isActive变为false
        i.isActive = false;
      });
      this.tabs[index].isActive = true;  //点击的改为true
      console.log(index,this.num)
      this.num=index+1   //因为index从0开始num从一开始
    },
    // 分类切换事件
    fun(num) {
      this.index = num;
    },
    // 获取缓存中的数据
    async getlist() {
      let res = await uni.getStorage({ key: "start" });
      console.log(res[1].data);
      this.goodslist=res[1].data
    }
  }
};
</script>

<style lang="scss" scoped>
.fenlei {
  background-color: #ccc;
  display: flex;
  justify-content: flex-start;
  padding: 20rpx;
  .item1 {
    width: 50rpx;
    height: 50rpx;
    background-color: #fff;
    text-align: center;
    line-height: 50rpx;
    font-size: 20rpx;
  }
  .item {
    font-size: 20rpx;
    text-align: center;
    line-height: 50rpx;
    background-color: #fff;
    margin-left: 20rpx;
    width: 100rpx;
    height: 50rpx;
  }
}
.border {
  border: orangered solid 2rpx;
}
// 商品
.goodsbox{
  .gooditem{
    display: flex;
    padding: 10rpx;
    .img{
      flex: 1;
      image{
        width: 100%;
        height: 200rpx;
      }
    }
    .goodinfo{
      flex: 3;
      padding:20rpx 0px;
      .title{
        font-size: 30rpx;
        color: #000;
      }
      .price{
        padding: 20rpx 0px;
        font-size: 25rpx;
        color: orangered;
      }
    }
  }
}
</style>
