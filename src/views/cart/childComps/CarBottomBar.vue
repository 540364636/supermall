<template>
  <div class="bottom-bar">
    <div class="check-all">
      <check-button class="check-a" 
      :isCheck="isAll"  
      @click.native="checkClick"/>
      <span>全选</span>
    </div>
    <div class="price">合计：{{totalPrice}}</div>
    <div class="defray" @click="goPay">去计算({{checkLength}})</div>
  </div>
</template>

<script>
  import CheckButton from 'components/content/checkButton/CheckButton.vue'
  import {mapGetters} from 'vuex'


  export default {
  name: 'CarBottomBar',
  components: { 
    CheckButton,
  }, 
  computed: {
    ...mapGetters(['cartList']),


    totalPrice() {
      return '￥' + this.cartList.filter(item => {
        return item.checked
      }).reduce((preValue, item) => {
        return preValue + item.price * item.count
      }, 0).toFixed(2)
    },
    checkLength() {
      return this.cartList.filter(item => {
        return item.checked
      }).length
    },
    isAll() {
      if (this.cartList.length === 0) return false

      for (let item of this.cartList) {
        if (!item.checked) {
          return false
        }
      }
      return true
    }
  },
  methods: {
    checkClick() {
       if (this.isAll) {
         this.cartList.forEach(item => {
           item.checked = false
         })
       } else {
         this.cartList.forEach(item => {
           item.checked = true
         })
       }
    },
    goPay() {
      if (!this.isAll) {
        this.$toast.show('请选择购买的商品')
      }

    }
  }
  }
</script>

<style scoped>
  .bottom-bar {
    height: 40px;
    background-color: #eee;
    position: relative;
    line-height: 40px;
    display: flex;
    
    font-size: 14px;
  }

  .check-all {
    display: flex;
    align-items: center;
  }

  .check-a {

    width: 20px;
    height: 20px;
    line-height: 20px;
    margin-right: 5px;
    margin-left: 10px;
  }
  .price {
    margin-left: 20px;
    flex: 1;
  }
  .defray {
    width: 90px;
    background-color: red;
    color: #fff;
    text-align: center;
  }
</style>
