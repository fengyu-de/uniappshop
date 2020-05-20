<template>
  <view>
    <!-- 获取收货地址 -->
    <view class="address_btn" v-if="datainfo.length==0">
      <button type="primary" plain @click="fn">获取收货地址</button>
    </view>
    <!-- 授权人的收货地址 -->
    <view class="address" v-if="datainfo.length!==0">
      <view class="name">
        <view class="username">收货人:{{datainfo[1].userName}}</view>
        <view class="telNumber">{{datainfo[1].telNumber}}</view>
      </view>
      <view
        class="cityName"
      >{{datainfo[1].provinceName+datainfo[1].cityName+datainfo[1].countyName+datainfo[1].detailInfo}}</view>
    </view>
    <!-- 购物车内容 -->
    <view class="cart_content">
      <view class="cart_title">购物车</view>
      <view class="cart_main" v-for="(item,index) in goodslist" :key="index">
        <!-- 复选框 -->
        <view class="cart_checkbox">
          <checkbox-group v-model="checkedlist">
            <checkbox :checked="item.checked" @click="nochecked(item.goods_id)"></checkbox>
          </checkbox-group>
        </view>
        <!-- 图片 -->
        <view class="cart_img">
          <image :src="item.goods_small_logo" mode="widthFix"></image>
        </view>
        <!-- 商品信息 -->
        <view class="cart_info">
          <view class="goods_name">{{item.goods_name}}</view>
          <view class="goods_price_box">
            <view class="goods_price">￥{{item.goods_price}}</view>
            <view class="goods_num">
              <view class="edit" @click="sunfun('s',item.goods_id)">-</view>
              <view class="num">{{item.num}}</view>
              <view class="edit" @click="sunfun('b',item.goods_id)">+</view>
            </view>
          </view>
        </view>
      </view>
    </view>
    <!-- 工具栏 -->
  <view class="footer_tool">
    <!-- 全选 -->
    <view class="all">
      <checkbox-group>
        <checkbox @click="allfun" :checked="all">全选</checkbox>
      </checkbox-group>
       </view>
       <!-- 总价格 -->
       <view class="all_price">
         <view class="price">
           合计&nbsp;&nbsp;:<text class="text">￥&nbsp;&nbsp;{{price}}</text>
           <view class="cart_price">包含运费</view>
         </view>
       </view>
       <view class="pay" @click="handlepay">
         结算({{num}})
       </view>
  </view>
  <view class="null" v-if="goodslist.length==0">
      你还没有商品快去选购吧
  </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      datainfo: [], //用户地址数据,
      goodslist:[],   //缓存中的商品列表数据
    };
  },
  methods: {
    // 获取收货地址事件
    async fn() {
      //获取 用户 //地理位置的方法
      let res = await uni.getLocation({});
      console.log(res);
      let res2 = await uni.chooseAddress();
      if (res2.length == 1) {
        uni.showToast({
          title: "取消授权"
        });
        return;
      }
      this.datainfo = res2;
      // 将用户地址缓存
      uni.setStorage({ ket: "userzhu", data: this.datainfo });
      console.log("用户信息",res2);
    },
    // 点击全选事件
    allfun(){
         this.goodslist.forEach(i=>{
        i.checked=true
      }
      )
    },
    // 商品的选中
   async nochecked(id){
      console.log(id)
     let list= this.goodslist.filter(v=>v.goods_id==id)  //根据id找到对应的商品书籍
     list[0].checked=!list[0].checked  //勾选取反
     if(list[0].checked==false){   //如果为false
      let goodlist2= this.goodslist.filter(v=>v.checked==true)  //重新过滤出新的商品列表数据
      uni.setStorage({key:"cart",data:goodlist2})   //重新将为true的商品列表数据存到缓存中
     let res=await uni.getStorage({key:"cart"})     //重新获取缓存中的数据
     if(res.length==1){
       res=[]
     }else{
      res=res[1].data
     }
     console.log("res",res)
     this.goodslist=res
     }
    },
    // 商品的增加删除
    sunfun(n,id){
     let index= this.goodslist.findIndex(v=>v.goods_id==id)
     if(n=="s"){
       this.goodslist[index].num--
       if( this.goodslist[index].num==0){
         uni.showToast({
           title:"商品数量不能小于1"
         })
       }
       return
     }
     this.goodslist[index].num++
     console.log(index,this.goodslist[index])
    } ,
    // 点击结算事件
    handlepay(){
    //  1.判断用户信息是否授权
    if(this.datainfo.length==0){
      uni.showToast({
      title:"你还没有选择地址"
    })
    return
    }
    // 2.在判断缓存中是否有商品列表数据
    if(this.goodslist.length==0){
      uni.showToast({
      title:"你还没有选择地址"
    })
    return
    }
    // 都通过跳转到支付页面
    uni.navigateTo({
      url:"/pages/pay/index"
    })
    }
  },
 
 async onShow(){
    // 获取缓存中的数据
   let res=await uni.getStorage({key:"cart"})
   if(res.length==1){
     res=[]
     uni.showToast({
       title:"还没有商品在购物车"
     })
     return
   }
   console.log("缓存",res)
   this.goodslist=res[1].data
   console.log(res[1].data)
   console.log("goodslist",this.goodslist)
  },
  computed:{
  all(){
    // every函数 数组的每一个项为true就返回true
    if(this.goodslist.length==0){
      return false
    }
   return this.goodslist.every(i=>i.checked)
  },
  price(){
    let prices=0
    this.goodslist.forEach(i=>{
     prices+=i.goods_price*i.num
    })
    return prices
  },
  num(){
    let num=0
    this.goodslist.forEach(v=>{
     num+=v.num
    })
    return num
  }
  }
};
// 获取用户地址权限逻辑(状态 scope)
// 1.假设用户点击获取收货地址的提示框 确定
//  scope值为 true 直接调用获取收货地址
// 2.假设用户从来没有调用过 获取收货地址
// scope 值为 undefined 直接调用获取收货地址
// 3.点击取消获取收货地址
// scope 值为 false
//  自己打开授权获取地址权限

// 商品的选中
// 1.绑定change事件
// 2.获取到被修改的商品对象
// 3.商品对象的选中状态取反
// 4.重新填充回data中和缓存中
// 5。重新计算全选。总价格 总数量

// 商品结算
// 1.判断有没有收货地址信息
// 2.判断用户有没有选商品
// 3.经过以上验证跳转到支付页面
</script>

<style lang="scss" scoped>
.address_btn {
  padding: 20rpx;
}
// 收货地址
.address {
  padding: 10rpx;
  display: flex;
  flex-wrap: wrap;
  border-bottom: 2rpx solid #ccc;
  .name {
    width: 100%;
    display: flex;
    padding: 10rpx;
    justify-content: space-between;
    .username {
      font-size: 28rpx;
    }
    .telNumber {
      font-size: 28rpx;
    }
  }
  .cityName {
    padding: 10rpx;
  }
}
// 购物车列表
.cart_content {
  .cart_title {
    padding: 20rpx;
    font-size: 36rpx;
    color: orangered;
    border-bottom: 1px solid orangered;
  }

  .cart_main {
    padding: 10rpx;
    width: 100%;
    display: flex;
    align-items: center;
    border-bottom: 1px solid #ccc;
    .cart_checkbox {
      display: flex;
      justify-content: center;
      flex: 1;
      padding: 5rpx;
      checkbox-group {
        checkbox {
          color: orangered;
        }
      }
    }

    .cart_img {
      display: flex;
      justify-content: center;
      flex: 2;
      padding-right: 10rpx;
      image{
        width: 50%;
      }
    }
    .cart_info {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 5rpx;
      flex: 2;
      .goods_name {
        // 超过两行。。。表示
        display: -webkit-box;
        overflow: hidden;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        color: #666;
      }

      .goods_price_box {
        width: 100%;
        display: flex;
        justify-content: space-between;
        margin-top: 40rpx;
        .goods_price {

            color: orangered;
            font-size: 36rpx;
        }

        .goods_num {
         display: flex;
         justify-content: space-between;
          .edit {
            width: 50rpx;
            text-align: center;
            line-height: 50rpx;
            height: 50rpx;
            border: 1px solid #ccc;
          }

          .num {
            margin: 0 8rpx;
          }
        }
      }
    }
  }
}
// 小工具
.footer_tool {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 90rpx;
  background-color: #ccc;
  display: flex;
  align-items: center;
  justify-content: center;
  .all {
    flex: 1;
    checkbox-group {
      checkbox {

      }
    }
  }

  .all_price {
    flex: 2;
    display: flex;
    justify-content: flex-end;
    .price {
      text.text {
        font-size: 30rpx;
        color: orangered;
      }

      .cart_price {
        display: flex;
        justify-content: flex-end;
      }
    }
  }

  .pay {
    flex: 1;
    height: 100%;
    background-color: orangered;
    text-align: center;
    line-height: 90rpx;
    font-size: 36rpx;
  }
}
// 提示文字
.null{
  width: 50%;
  position: absolute;
  bottom: 60%;
  left:50%;
  transform: translate(-50%,-50%);
  color: #ccc;
  font-size: 40rpx;

}
</style>
