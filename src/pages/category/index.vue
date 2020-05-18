<template>
  <view></view>
</template>

<script>
export default {
  data() {
    return {
      categorieslist: [] //商品分类数据
    };
  },
  mounted() {
    this.getlist(); //获取分类数据
  },
  methods: {
    async getlist() {
      let res = await uni.request({
        url: "https://api-hmugo-web.itheima.net/api/public/v1/categories"
      });
      this.categorieslist = res[1].data.message;
      console.log(res[1].data.message);
    }
  }
};
</script>

<style>
</style>
