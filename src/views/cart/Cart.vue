<template>
  <div class="cart">
    <nav-bar class="nav-bar">
      <div slot="center">购物车({{cartLength}})</div>
    </nav-bar>
    <scroll class="content" ref="scroll">
      <cart-list/>
    </scroll>
    <car-bottom-bar/>
  </div>
  
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar.vue'
  import CartList from './childComps/CartList.vue'
  import Scroll from 'components/common/scroll/Scroll.vue'
  import CarBottomBar from './childComps/CarBottomBar.vue'


  import {mapGetters} from 'vuex'

  export default {
    name: 'Cart',
    components: {
      NavBar,
      CartList,
      Scroll,
      CarBottomBar,
    },
    computed: {
      ...mapGetters(['cartLength'])
    },
    activated() {
      this.$refs.scroll.refresh()
    }
  }
</script>

<style scoped>
  .cart {
    height: 100vh;
  }
  .nav-bar {
    background-color: var(--color-tint);
    color: #fff;

  }

  .content {
    height: calc(100% - 44px - 49px - 40px);
    overflow: hidden;
  }
</style>
