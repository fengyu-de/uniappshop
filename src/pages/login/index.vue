<template>
  <view>
    <button type="primary" plain open-type="getUserInfo" @getuserinfo="getuserinfo">登录</button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      userInfo: []
    };
  },
  methods: {
    getuserinfo(e) {
      console.log(e);
      let { detail } = e;
      console.log(detail.userInfo);
      this.userInfo.push(detail.userInfo);
      // 将用户数据放入缓存中
      uni.setStorage({ key: "user", data: this.userInfo });
      // 再自动跳转用户页面
      uni.navigateBack({
        delta: 1 //返回上一页
      });
    }
  }
};
</script>

<style>
</style>
