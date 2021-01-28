<template>
  <div>
    <img :src="'https://images.weserv.nl/?url=' + tvObj.cover.image.small.url" alt="" />
    <h1>{{ tvObj.title }}</h1>
    <p>{{ tvObj.intro }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tvObj: {},
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      let baseUrl = "https://bird.ioliu.cn/v2?url=";
      let tvUrl =
        "https://m.douban.com/rexxar/api/v2/tv/" +
        this.$route.params.id +
        "?ck=&for_mobile=1";
      this.axios
        .get(baseUrl + tvUrl)
        .then((res) => {
          console.log(res);
          this.tvObj = res.data;
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style lang="scss" scoped></style>
