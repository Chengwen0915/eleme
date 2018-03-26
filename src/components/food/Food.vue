<template>
  <transition name="fade">
    <div class="food" v-show="showFood" ref="foodWrapper">
      <div class="img-box">
        <img :src="food.image" alt="">
        <i class="iconfont icon-right-arrow" @click="goBack"></i>
      </div>

      <div class="food-info">
        <h1 class="name">{{food.name}}</h1>
        <p class="sell-info"><span class="sellCount">月售{{food.sellCount}}份</span><span
          class="good-rating">好评率{{food.rating}}%</span></p>
        <p class="price-info">
        <span class="price">
          <span>¥</span>
          <span>{{food.price}}</span>
        </span>

          <span class="old-price" v-show="food.oldPrice">
          <span>¥</span>
          <span>{{food.oldPrice}}</span>
        </span>
        </p>
        <div class="cart-control" v-show="food.count > 0">
          <cartControl :food="food"></cartControl>
        </div>

        <input @click.stop.prevent="addFirst" type="button" v-show="!food.count || food.count === 0" class="btn" value="加入购物车">

      </div>

      <p class="insulate"></p>

      <div class="sellerDesc" v-show="food.info">
        <h2 class="text">商家介绍</h2>
        <div class="desc">{{food.info}}</div>
      </div>
      <p class="insulate" v-show="food.info"></p>

      <div class="rating">
        <h2 class="text">商品评价</h2>

      </div>
    </div>
  </transition>
</template>

<script>
  import bScroll from 'better-scroll';
  import Vue from 'vue';
  import cartControl from '../cartControl/CartControl'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data(){
      return {
        showFood: false
      }
    },
    components:{
      cartControl
    },
    methods: {
      show(){
        this.showFood = true;
        this.$nextTick( () => {
            if(!this.foodScroll){
              this.foodScroll = new bScroll(this.$refs.foodWrapper, {
                click: true
              });
            }else{
                this.foodScroll.refresh();
            }
        })
      },
      goBack(){
        this.showFood = false;
      },
      addFirst(){
          if(!this.food.count){
              Vue.set(this.food,"count",1)
          }
      }
    }
  }
</script>

<style scoped lang="less">
  .food {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 48px;
    width: 100%;
    z-index: 30;
    background-color: #fff;
    overflow: hidden;
    .img-box {
      position: relative;
      width: 100%;
      img {
        width: 100%;
        height: auto;
      }
      .iconfont{
        position: absolute;
        top: 10px;
        left: 10px;
        font-size: 20px;
        color: #fff;
        transform: rotate(180deg);
      }
    }
    .food-info {
      position: relative;
      width: 100%;
      padding: 18px;
      box-sizing: border-box;
      .name {
        font-weight: 700;
        color: rgb(7, 17, 27);
        line-height: 14px;
        font-size: 14px;
      }
      .sell-info {
        position: relative;
        margin: 8px 0 18px 0;
        .sellCount, .good-rating {

          line-height: 10px;
          font-size: 10px;
          color: rgb(147, 153, 159);
        }
        .sellCount {
          margin-right: 12px;
        }
      }
      .price-info{
        .price {
          span {
            color: rgb(240, 20, 20);
            line-height: 24px;
          }
          span:first-of-type {
            font-size: 10px;
            font-weight: normal;
          }
          span:last-of-type {
            font-size: 14px;
            font-weight: 700;

          }
        }
        .old-price{
          text-decoration: line-through;
          span {
            font-size: 10px;
            color: rgb(147, 153, 159);
            line-height: 24px;
          }
          span:first-of-type {
            font-weight: normal;
          }
          span:last-of-type {
            font-weight: 700;

          }
        }
      }
      .cart-control{
        position: absolute;
        right: 18px;
        bottom: 18px;
      }
      .btn{
        position: absolute;
        right: 18px;
        bottom: 18px;
        width: 74px;
        height: 24px;
        border:none;
        outline: none;
        border-radius: 12px;
        font-size: 10px;
        line-height: 12px;
        background-color: rgb(0,160,220);
        color: #fff;
      }

    }
    .insulate{
      width: 100%;
      height: 16px;
      background-color: #f3f5f7;
      border-top:1px solid rgba(7,17,27,.1);
      border-bottom:1px solid rgba(7,17,27,.1);
    }
    .sellerDesc{
      position: relative;
      width: 100%;
      padding: 18px;
      box-sizing: border-box;
      .text{
        font-weight: 200;
        line-height: 14px;
        font-size: 14px;
        color: #07111b;
        margin-bottom: 6px;
      }
      .desc{
        font-weight: 200;
        color: rgb(77, 85, 93);
        line-height: 24px;
        font-size: 12px;
      }
    }
    .rating{

    }
  }
  .fade-enter-active,.fade-leave-active{
    transition: all 0.2s ease;
  }
  .fade-enter,
  .fade-leave-to{
    transform: translateX(100%);
    opacity: 0;
  }

</style>
