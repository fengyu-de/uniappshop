<template>
  <view>
    <!-- 收缩区域 -->
    <view class="search">
      <input v-model="text" type="text" class="input" placeholder="请输入要搜索的商品名称" @input="func" />
      <button class="btn" size="mini" @click="stop" v-if="goodlist.length>0">取消</button>
    </view>
    <!-- 搜索结果区域 -->
    <view class="goodlist">
      <view
        @click="godetail(item.goods_id)"
        class="item"
        v-for="item in goodlist"
        :key="item.goods_id"
      >{{item.num+1}}.&nbsp;{{item.goods_name}}</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      text: "", //搜索的参数
      goodlist: [], //获取到的数据
      id: null //控制定时器id
    };
  },
  methods: {
    async fn() {
      console.log(this.text);
      // 请求数据
      let res = await uni.request({
        url: "https://api-hmugo-web.itheima.net/api/public/v1/goods/qsearch",
        data: { query: this.text }
      });
      console.log(res);
      // 给获取的数据对象中添加一个数字（序号）
      for (let i = 0; i < res[1].data.message.length; i++) {
        res[1].data.message[i].num = i;
      }
      this.goodlist = res[1].data.message;
      console.log(this.goodlist.num);
    },
    // 实现防抖
    func() {
      clearTimeout(this.id); //先清除延时器
      this.id = setTimeout(() => {
        //再各多少秒执行请求函数
        this.fn();
      }, 2000);
    },
    // 取消搜索
    stop() {
      this.goodlist = [];
      // 输入框的值清空
      this.text = "";
    },
    godetail(id) {
      uni.navigateTo({
        url: `/pages/good_detail/index?id=${id}`
      });
    }
  }
};
// 防抖的实现
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  padding: 20rpx;
  height: 80rpx;
  background-color: #666;
  .input {
    flex: 5;
    background-color: #fff;
    height: 100%;
  }
  .btn {
    flex: 1;
    text-align: center;
    line-height: 80rpx;
    margin-left: 10rpx;
  }
}
// 商品收缩区域
.goodlist {
  padding: 10rpx;
  .item {
    border-bottom: 2rpx solid #666;
    text-align: left;
    font-size: 30rpx;
    color: rosybrown;
    padding: 10rpx 2rpx;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    // line-height: ;
  }
  .item:hover {
    background-color: peachpuff;
  }
}
</style>
