<template>
  <view>
    <Tabs :tabs="tabs" @func="fun"></Tabs>
    <view>{{tabs[index].title}}</view>
  </view>
</template>

<script>
import Tabs from "@/components/tabs/index"; //导入tabs切换组件
export default {
  data() {
    return {
      tabs: [
        //导航数据
        {
          id: 0,
          title: "全部",
          isActive: false
        },
        {
          id: 1,
          title: "待付款",
          isActive: false
        },
        {
          id: 2,
          title: "待发货",
          isActive: false
        },
        {
          id: 3,
          title: "退款/退货",
          isActive: false
        }
      ],
      type: 0, //传入的数据,
      index: 0
    };
  },
  onLoad(e) {
    console.log(e);
    this.type = e.type;
  },
  mounted() {
    this.fn(); //更改tabs颜色
    this.getlist(); //获取订单数据
  },
  components: {
    Tabs
  },
  methods: {
    fun(index, title) {
      this.tabs.forEach(i => (i.isActive = false)); //先把数组中的isActive改为false
      this.tabs[index].isActive = true; //再将点击的改为true
      this.index = index;
    },
    fn() {
      if (this.type == 1) {
        //判断传递过来的值控制选择的导航
        this.tabs[0].isActive = true;
        this.index = 0;
      } else if (this.type == 2) {
        this.tabs[1].isActive = true;
        this.index = 1;
      } else {
        this.tabs[2].isActive = true;
        this.index = 2;
      }
    },
    // 获取商品订单数据
    async getlist() {
      let res = await uni.request({
        url: "https://api.zbztb.cn/api/public/v1/my/orders/all",
        data: this.type
      });
      console.log(res);
    }
  }
};
</script>

<style>
</style>
