<template>
  <div id="product">
    <div class="item-button">
      <check-button :isCheck="product.checked" @click.native="check"/>
    </div>
    <div class="image">
      <img :src="product.image" alt="">
    </div>
    <div class="item-info">
      <div class="info-title">{{product.title}}</div>
      <div class="info-desc">{{product.desc}}</div>
      <div class="info-bottom">
        <div class="price left">{{product.price}}</div>
        <div class="count right">{{product.count}}</div>
      </div>
    </div>
    
  </div>
</template>

<script>
  
  import CheckButton from 'components/content/checkButton/CheckButton.vue'

  export default {
    name: 'CartListItem',
    components: {
      CheckButton,
    },
    props: {
      product: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    methods: {
      check() {
        this.product.checked = !this.product.checked
      }
    }
  }
</script>

<style scoped>
  #product {
    display: flex;
    padding: 5px;
    width: 100%;
    font-size: 0;
    border-bottom: 1px solid #ccc;
  }

  .item-button {
    width: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .image {
    padding: 5px;
  }

  .image img {
    display: block;
    height: 100px;
    width: 80px;
    border-radius: 5px;
  }

  .info-title, .info-desc {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  .info-desc {
    color: #666;
    font-size: 14px;
    margin-top: 15px;
  }
  .item-info {
    position: relative;
    padding: 5px 10px;
    font-size: 17px;
    color: #333;
    overflow: hidden;
  }

  .info-bottom {
    position: absolute;
    bottom: 10px;
    left: 10px;
    right: 10px;
    margin-top: 10px;
  }
  .price {
    color: orangered;
  }
</style>
