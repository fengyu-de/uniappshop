<template>
  <view class="tabs">
    <view
      :class="['tabs_title',{'color':item.isActive}]"
      v-for="(item,index) in tabs"
      :key="item.id"
      @click="fun(index,item.title)"
    >
      <view class="title_item">{{item.title}}</view>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    tabs: Array
  },
  methods: {
    fun(index, title) {
      console.log(this.tabs);
      console.log("点击", index, title);
      this.$emit("func", index, title);
    }
  }
};
</script>

<style lang="scss">
.tabs {
  display: flex;
  justify-content: space-around;
  .tabs_title {
    padding: 15rpx;
    flex: 1;
    .title_item {
      text-align: center;
      font-size: 30rpx;
    }
  }
}
.color {
  color: orangered;
  border-bottom: 3px solid pink;
}
</style>
