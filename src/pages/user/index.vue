<template>
  <view>
    <!-- 用户头像 -->
    <view class="user_wrap">
      <view class="user_img">
        <image  class="user_bg" :src="userinfo[0].avatarUrl" ></image>
        <view class="user_info">
          <image mode="widthFix" class="user_icon" :src="userinfo[0].avatarUrl"></image>
          <view class="user_name">{{userinfo[0].nickName}}</view>
        </view>
      </view>
    </view>
    <!-- 小头像 -->
    <view class="login" v-if="userinfo.length==0">
      <button type="primary" @click="login">登录</button>
    </view>
    <!-- 工具 -->
    <view class="user_content">
      <view class="top_nav">
        <view class="nav_item" @click="gofun(1)">
          <view class="num">{{startnum}}</view>
          <view class="text" >收藏的店铺</view>
        </view>
        <view class="nav_item"  @click="gofun(2)">
          <view class="num">0</view>
          <view class="text">收藏的商品</view>
        </view>
        <view class="nav_item"  @click="gofun(3)">
          <view class="num">0</view>
          <view class="text">关注的商品</view>
        </view>
        <view class="nav_item"  @click="gofun(4)">
          <view class="num">0</view>
          <view class="text">我的足迹</view>
        </view>
      </view>
      <view class="dindan">
        我的订单
      </view>
      <view class="nav">
        <view class="item" @click="fun(1)">
          <i class="iconfont icon-dingdan "></i>
          <view class="text">全部订单</view>
        </view>
        <view class="item" @click="fun(2)">
          <i class="iconfont icon-fukuantongzhi"></i>
          <view class="text">待付款</view>
        </view>
        <view class="item" @click="fun(3)">
          <i class="iconfont icon-gouwuchekong "></i>
          <view class="text">待收货</view>
        </view>
        <view class="item">
          <i class="iconfont icon-fukuantongzhi "></i>
          <view class="text">退款/退款</view>
        </view>
      </view>
      <view class="shouhuo">
        收货地址管理
      </view>
      <view class="kefu">
       <view> 联系客服</view>
       <view> 400-618-4000</view>
      </view>
      <view class="yijian" @click="goyijian"> 
        意见反馈
      </view>
      <view class="about">
        关于我们
      </view>
      <view class="tuijian">
        把应用推荐给其他人
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data(){
    return{
      userinfo:[], //用户信息
      startnum:0
    }
  },
  mounted() {
    this.getuserinfo()   //获取用户信息
  },
  onShow(){
    this. getstartclist()     //获取缓存中的收藏数据
  },
  methods:{
   async getuserinfo(){
    let res=await uni.getStorage({key:"user"})
    console.log("res",res)
    if(res.length==1){
      uni.showToast({
        title:'还没有授权'
      })
      return
    }
    this.userinfo=res[1].data
    uni.navigateTo({
      url:"pages/user/index"
    })
   console.log(this.userinfo,this.userinfo.length)
    },
    // 登录时间
    login(){
      // 如果缓存中没有用户数据就跳转
      uni.navigateTo({
        url:"/pages/login/index"
      })
    },
    // 跳转订单页面事件
    fun(id){
        uni.navigateTo({
          url:`/pages/order/index?type=${id}`
        })
    },
    // 获取缓存中收藏的商品数据
   async getstartclist(){
    let res=await  uni.getStorage({key:"start"})
    this.startnum=res[1].data.length
    },
     // 跳转收藏页面
  gofun(num){
    uni.navigateTo({
      url:`/pages/collect/index?num=${num}`
    })
  },
  // 跳转意见反馈页面
  goyijian(){
    uni.navigateTo({
      url:"/pages/feedback/index"
    })
  }
  }
 

  
};
</script>

<style lang="scss" scoped>
.user_wrap {
  height: 45vh;
  border-bottom: 2rpx solid #ccc;
  .user_img {
        position: relative;
        height: 45vh;
    image{
        width:100% ;
        height: 100%;
        filter: blur(5px);  //图片模糊效果
    }

    .user_info {
      position: absolute;
      left:50%;
      transform: translateX(-50%);
      top: 20%;
      image{
        filter: blur(0);
        width: 200rpx;
        height: 200rpx;
        
        border-radius: 50%;
     }
      img.user_icon {
        
      }

      .user_name {
        margin-top: 20rpx;
        text-align: center;
        color: #fff;
        font-size: 30rpx;
      }
    }
  }
}
// 登录按钮
.login{
  position: absolute;
  left: 50%;
  top:30%;
  transform: translateX(-50%);
  button{

  }
}
// 用户内容
.user_content{
  background-color: #ccc;
  height:  55vh;
  .top_nav{
    position: absolute;
    width: 80%;
    left: 10%;
    bottom:55%;
    background-color: #fff;
    display: flex;
    .nav_item{
      flex: 1;
      padding: 15rpx;
      .num{
        text-align: center;
        color: orange;
        font-size: 20rpx;
      }
      .text{
        text-align: center;
        font-size: 25rpx;
      }
    }
  }
  .dindan{
    margin-top: 30rpx;
    background-color: #fff;
    font-size: 30rpx;
    padding: 15rpx;
    border-bottom: 2rpx solid #ccc;
  }
  .nav{
    margin-top: 10rpx;
    display: flex;
    background-color: #fff;
    .item{
      flex: 1;
      .text{
        text-align: center;
      }
      .iconfont{
        text-align: center;
        color: orangered;
      }
    }
  }
  .shouhuo{
    background-color: #fff;
    padding: 15rpx;
    margin-top: 10rpx;
  }
  .kefu{
    display: flex;
    justify-content: space-between;
    background-color: #fff;
    margin-top: 15rpx;
    padding: 10rpx;
  }
  .yijian{
    background-color: #fff;
    margin-top: 5rpx;
    padding: 15rpx;
  }
  .about{
    background-color: #fff;
    margin-top: 5rpx;
    padding: 15rpx;
  }
  .tuijian{
    background-color: #fff;
    margin-top: 10rpx;
    padding: 20rpx;
  }
}
</style>

