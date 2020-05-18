<template>
  <view>
    <!-- 搜索 -->
    <serchInput></serchInput>
    <!-- 商品分类（左侧） -->
    <view class="category_content">
      <scroll-view class="left_menu" scroll-y>
        <view @click="fun(index)" :class="['menu_item',{'click':index==i}]" v-for="(item,index) in leftMenuList" :key="index">{{item}}</view>
      </scroll-view>
      <!-- 商品分类(右侧) -->
      <scroll-view class="right_menu" scroll-y>
        <view class="goods_group" v-for="item in rightMenuList" :key="item.cat_id">
          <view class="goods_title">{{item.cat_name}}</view>
          <view class="goods_list">
            <view v-for="item2 in item.children" :key="item2.cat_id" @click="fn(item2.cat_id)">
              <image :src="item2.cat_icon" mode="widthFix" ></image>
            </view>
          </view>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
import serchInput from "@/components/serchinput/index";
export default {
  data() {
    return {
      rightMenuList: [], //右侧商品数据
      leftMenuList: [], //左侧菜单数据
      categorieslist: [],//接口返回数据
      i:0               //记录当前点击的索引
    };
  },
  mounted() {
    
  },
   async created(){
  //数据缓存
  // 判断本地存储中是否有数据如果有就使用旧数据，如果没有就重新获取数据
  // 1.获取本地存储
  const list=await uni.getStorage({key:"cates"})
  console.log("list",list)
  // 2.判断是否存在 不存在就重新获取
  if(list.length==1){
    this.getlist(); //获取分类数据
  }else{
    this.categorieslist=list[1].data
    this.leftMenuList = this.categorieslist.map(v => v.cat_name); //将获取到的数据中的cat_name赋值到左侧菜单
    this.rightMenuList = this.categorieslist.map(i => i.children[this.i]); //将获取到的数据中的children赋值到左侧菜单
  }
  },
  methods: {
    async getlist() {
      let res = await uni.request({
        url: "https://api-hmugo-web.itheima.net/api/public/v1/categories"
      });
      this.categorieslist = res[1].data.message;
      // 将数据存到本地存储中
      await uni.setStorage({key:"cates",data:res[1].data.message})
      console.log(this.categorieslist)
      this.leftMenuList = this.categorieslist.map(v => v.cat_name); //将获取到的数据中的cat_name赋值到左侧菜单
      this.rightMenuList = this.categorieslist.map(i => i.children[this.i]); //将获取到的数据中的children赋值到左侧菜单
      console.log(this.leftMenuList, this.rightMenuList);
      console.log(res[1].data.message);
    },
    fun(index){
      this.i=index
      this.rightMenuList = this.categorieslist.map(i => i.children[this.i])  //重新将rightMenuList赋值(索引显示商品不同内容)
    },
    // 跳转到商品列表
    fn(id){
      uni.navigateTo({
        url:`/pages/good_list/index?id=${id}`  //点击图片传入的id
      })
    }
  },
  components: {
    serchInput
  }
};
</script>

<style lang="scss" scoped>
.category_content {
  // height: 100%;
  display: flex;
  // 左侧菜单
  .left_menu {
    flex: 2;
    height: calc(100vh - 90rpx); //scroll-view的滚动区域
    .menu_item {
      height: 60rpx;
      text-align: center;
      line-height: 60rpx;
    }
  }
  // 右侧菜单
  .right_menu {
    flex: 5;
    height: calc(100vh - 90rpx);
    .goods_group{
      .goods_title{
        width: 100%;
        text-align: center;
        line-height: 80rpx;
        color: #666;
        padding: 0px 10rpx;
        height: 80rpx;
      }
      .goods_list{
        display: flex;
        flex-wrap: wrap;
        justify-content: flex-start;
        width: 100%;
        view{
          text-align: center;
          width: 33.3%;
          image{
            width: 80%;
          }
        }
      }
    }
  }
}
// 点击字体颜色
.click{
  color: orange;
}
</style>
