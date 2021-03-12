<template>
  <div class = "detailInfo">
    <div class = "desc">{{detailInfo.desc}}</div>
    <div class = "img-box">
      <img class="detailImage" v-for="(item, index) in detailInfo.detailImage"
           :src="item" @load="imageLoad" :key="index">
    </div>
  </div>
</template>

<script>
  export default {
    name: "DetailGoodsInfo",
    props: {
      detailInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    methods: {
      imageLoad() {
        this.$emit("detailImageLoad")
      }
    }
  }
</script>

<style scoped>
  .img-box {
    width: 100%;
  }

  .desc {
    box-shadow: 0 -15px 15px rgba(100, 100, 100, 0.3);
    padding: 10px 20px;
    margin-top: 20px;
    line-height: 22px;
    font-size: var(--font-size);
  }

  .detailImage {
    width: 100%;
    height: auto;
  }
</style>
