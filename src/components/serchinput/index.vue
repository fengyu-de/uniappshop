<template>
  <!-- 搜索组件 -->
  <view class="search_input">
    <!-- 跳转组件 -->
    <navigator url="/pages/search/index">搜索</navigator>
  </view>
</template>

<script>
export default {};
</script>

<style lang="scss">
.search_input {
  height: 90rpx;
  padding: 10rpx;
  background-color: #eb4450;
  navigator {
    text-align: center;
    color: #ccc;
    height: 100%;
    line-height: 90rpx;
    background-color: azure;
    border-radius: 15rpx; //边框圆角
  }
}
</style>
