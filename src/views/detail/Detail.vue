<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"/>
    <scroll class="content" ref="scroll" @scroll="contentScroll" :probe-type="3">
      <detail-swiper :topImages="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detailInfo="detailInfo" @detailImageLoad="detailImageLoad"/>
      <detail-param-info :paramInfo="paramInfo" ref = "params"/>
      <detail-comment-info :commentInfo="commentInfo" ref = "comment"/>
      <goods-list :goods="recommend" ref = "recommend"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
    <detail-bottom-bar @addCart="addCart"/>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import DetailShopInfo from './childComps/DetailShopInfo'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
  import DetailParamInfo from './childComps/DetailParamInfo.vue'
  import DetailCommentInfo from './childComps/DetailCommentInfo.vue'
  import GoodsList from '../../components/content/goods/GoodsList.vue'
  import DetailBottomBar from './childComps/DetailBottomBar.vue'

  import Scroll from 'components/common/scroll/Scroll'
  
  import {debounce} from "common/utils"
  import {itemListenerMixin, backTopMixin} from 'common/mixin'
  import {getDetail, Goods, Shop, GoodsParam, getRecommend} from 'network/detail'

  export default {
    name: 'Detail',
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      Scroll,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      GoodsList,
      DetailBottomBar,
    },
    mixins: [itemListenerMixin, backTopMixin],
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommend: [],
        themeTopYs: [],
        getThemeTopY: null,
        currentIndex: 0,
      }
    },
    created() {

      this.iid = this.$route.query.iid
      getDetail(this.iid).then(res => {
        const data = res.result

        this.topImages = data.itemInfo.topImages

        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

        this.shop = new Shop(data.shopInfo)

        this.detailInfo = {
          desc: data.detailInfo.desc,
          detailImage: data.detailInfo.detailImage[0].list
        }

        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

        if (data.rate.cRata !==0) {
          this.commentInfo = data.rate.list[0]
        }
      })

      getRecommend().then(res => {
        this.recommend = res.data.list
      })

      this.getThemeTopY = debounce(() => {
        this.themeTopYs.push(0)
        this.themeTopYs.push(this.$refs.params.$el.offsetTop)
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
        this.themeTopYs.push(Number.MAX_VALUE)
      }, 100)
    },
    methods: {
      detailImageLoad() {
        this.newRefresh()
        this.getThemeTopY()
        
      },
      titleClick(index) {
        this.$refs.scroll.scroll.scrollTo(0, -this.themeTopYs[index], 500)
      },
      
      contentScroll(position) {
        this.isShowBackTop = (-position.y) > 1000

        for(let i = 0; i < this.themeTopYs.length; i++) {
          if (this.currentIndex !== i && (-position.y > this.themeTopYs[i] && -position.y < this.themeTopYs[i+1]))  {
            this.currentIndex = i
            this.$refs.nav.currentIndex = this.currentIndex
          }
        }
      },
      addCart() {
        const product = {}
        product.iid = this.iid
        product.image = this.topImages[0]
        product.title = this.goods.title
        product.desc = this.goods.desc
        product.price = this.goods.realPrice

        this.$store.dispatch('addCart', product).then(res => {
          this.$toast.show(res, 1500)
        })
      }
    },
    destroyed() {
      this.$bus.$off('itemImgLoad', this.itemImgListener)
    }
    
  }
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    /* background-color: #fff; */
  }

  .content {
    height: calc(100% - 44px - 58px);
  }
</style>
