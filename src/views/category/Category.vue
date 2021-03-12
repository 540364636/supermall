<template>
  <div>
    <category-nav-bar/>
    <scroll class="cate-box" ref="scrollLeft">
      <cate-box :wares="wares" @loaded="leftLoaded" @indexChange="indexChange"/>
    </scroll>
    <scroll class="items-box" ref ="scrollRight">
      <goods-list :goods="currentCate"/>
    </scroll>
  </div>
</template>

<script>
import Scroll from 'components/common/scroll/Scroll.vue'
import GoodsList from "components/content/goods/GoodsList";
import CateBox from './childComps/CateBox.vue'
import CategoryNavBar from './childComps/CategoryNavBar.vue'

import {getCategory, getSubcategory} from "network/category";

  export default {
    name: "Category",
    components: {
      Scroll,
      CategoryNavBar,
      CateBox,
      GoodsList
    },
    data() {
      return {
        wares: [],
        currentIndex: 0,
        currentCate: []
      }
    },
    created() {
      getCategory().then(res => {
        this.wares = res.data.category.list;
        this.changeCate()
      });
    },
    methods: {
      leftLoaded() {
        this.$refs.scrollLeft.refresh()
      },
      indexChange(index) {
        //index改变
        this.currentIndex = index;
        this.changeCate()
      },
      //种类变更
      changeCate() {
        getSubcategory(this.wares[this.currentIndex].maitKey).then(res => {
          this.currentCate = res.data.list;
          this.$refs.scrollRight.backTop()
        })
      }
    }
  }
</script>

<style scoped>
 .cate-box {
    position: absolute;
    top: 44px;
    left: 0;
    height: calc(100vh - 44px - 49px);
    width: 70px;
    background-color: #f4f4f4;
    overflow: hidden;
  }

  .items-box {
    position: absolute;
    top: 44px;
    left: 70px;
    width: calc(100vw - 70px);
    height: calc(100vh - 44px - 49px);
    overflow: hidden;
  }
</style>
