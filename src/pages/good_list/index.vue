<template>
  <view>
    <!-- 搜索 -->
    <searchInput></searchInput>
    <!-- 导航 -->
    <Tabs :tabs="tabs" @func="fun"></Tabs>
    <!-- 商品内容 -->
    <view class="good_content" v-if="index==0">
      <view class="first_tab">
        <navigator class="goods_item" v-for="item in goodslist" :key="item.cat_id">
          <!-- 左侧图片容器 -->
          <view class="goods_img">
            <image :src="item.goods_small_logo" mode="widthFix"></image>
          </view>
          <!-- 左侧商品介绍容器 -->
          <view class="goods_info">
            <view class="goods_name">{{item.goods_name}}</view>
            <view class="goods_price">￥{{item.goods_price}}</view>
          </view>
        </navigator>
      </view>
    </view>
    <view class="good_content" v-if="index==1">3</view>
    <view class="good_content" v-if="index==2">4</view>
  </view>
</template>

<script>
import searchInput from "@/components/serchinput/index";
import Tabs from "@/components/tabs/index";
export default {
  data() {
    return {
      tabs: [
        {
          id: 0,
          title: "综合",
          isActive: true //是否激活选中
        },
        {
          id: 1,
          title: "销量",
          isActive: false //是否激活选中
        },
        {
          id: 2,
          title: "价格",
          isActive: false //是否激活选中
        }
      ],
      index: 0,
      params:{ //请求参数
        query:"",
        cid:"",
        pagenum:1,
        pagesize:10
      },
      goodslist:[]   //请求的商品数据
    };
  },
  onLoad(e) {
    //接收上个页面传递过来的参数(id)
    console.log(e.id);
    this.params.cid=e.id
  },
  mounted(){
    this.getlist()  //获取商品列表
  },
  methods: {
    fun(index, title) {
      this.index = index;
      //接收子组件传递过来的参数
      // console.log("子组件的数据", index, title);
      this.tabs.forEach(i => {
        //将所有的 isActive设置为fales
        i.isActive = false;
      });
      this.tabs[index].isActive = true; //将子组件传递过来的索引修改为true
    },
   async getlist(){
     uni.showLoading({
       title:"数据加载中",
     })
    let res=await uni.request({
        url:"https://api-hmugo-web.itheima.net/api/public/v1/goods/search",
        data:this.params
      })
      if(res[1].data.message.goods.length==0){
          uni.showToast({
            title:"没有数据了",
            icon:"none"
          })
          return
      }
      // 数组的拼接扩展运算符
      this.goodslist=[...this.goodslist,...res[1].data.message.goods]
      uni.hideLoading()  //隐藏价值动画
      console.log(res)
    }
  },
  // 滚动条触底事件
  onReachBottom(){
    // 页码值加一
    this.params.pagenum+=1
    // 重新调用
    this.getlist()
  },
  // 下拉刷新事件
  onPullDownRefresh(){
    this.goodslist=[]  //重置数组
    this.params.pagenum=1  //页码重置
    this.getlist()  //从新调用
      uni.stopPullDownRefresh()  //关闭下拉刷新动画
  },
  components: {
    searchInput,
    Tabs
  }
};
</script>

<style lang="scss" scoped>
.good_content{
  padding: 20rpx;
  .first_tab{
    .goods_item{
      display: flex;
      width: 100%;
      border-bottom:1px solid #ccc ;
      .goods_img{
        flex: 1;
        image{
          width: 100%;
        }
      }
      .goods_info{
        flex: 2;
        padding: 20rpx;
        .goods_name{
          font-size: 30rpx;
          color: #000;
         
        }
         .goods_price{
            margin-top: 30rpx;
            font-size: 30rpx;
            color: orangered;
          }
      }
    }
  }
}
// 下拉刷新
// 1.触发下拉刷新事件
// 2.重置数据 数组
// 3.重置页码，
// 4.从新发送请求
</style>
