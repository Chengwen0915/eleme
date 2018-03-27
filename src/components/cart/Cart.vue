<template>
  <div class="cart">
    <div class="content" >
      <div class="content-left" >
        <div class="logo-wrapper" @click="toggleList">
          <div class="logo" :class="{'highlight':totalCount > 0}">
            <i class="iconfont icon-cart" :class="{'highlight':totalCount > 0}"></i>
          </div>
          <div class="num" :class="{'highlight':totalCount > 0}" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalCount > 0}">¥{{totalPrice}}</div>
        <div class="desc">另需要配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass" @click="pay">
          {{payDesc}}
        </div>
      </div>
    </div>

    <transition name="slider">
      <div class="cart-list" v-show="listShow">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty" @click="removeAll">清空</span>
        </div>
        <div class="list-content" ref="listWrapper">
          <ul class="list-box">
            <li class="list-food" v-for="(food,index) in selectGoods" :key="index">
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span>¥</span>
                <span>{{food.price * food.count}}</span>
              </div>
              <span class="cartcontrol-wrapper">
             <cartControl :food="food"></cartControl>
           </span>
            </li>
          </ul>
        </div>
      </div>
    </transition>
    <transition name="fade">
      <div class="list-mark" v-show="listShow" @click="hideList"></div>
    </transition>

  </div>

</template>

<script>
  import BScroll from 'better-scroll';
  import cartControl from '../cart-control/CartControl'
  export default {
//props: ["deliveryPrice", 'minPrice', 'selectGoods'],
    props: {
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
      selectGoods: {
        type: Array,
        default(){
          return []
        }
      }
    },
    data(){
      return {
        fold: true
      }
    },
    computed: {
      totalPrice(){
        let total = 0;
        for (let i = 0; i < this.selectGoods.length; i++) {
          total += this.selectGoods[i].price * this.selectGoods[i].count;
        }
//        total += this.deliveryPrice;
        return total;
      },
      totalCount(){
        let count = 0;
        this.selectGoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc(){
        if (this.totalPrice === 0) {
          return `¥${this.minPrice}起送`
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差¥${diff}起送`
        } else {
          return '去结算'
        }
      },
      payClass(){
        if (this.totalPrice < this.minPrice) {
          return 'not-enough'
        } else {
          return 'enough'
        }
      },
      listShow(){
        if (!this.totalCount) {
          this.fold = true;
        } else {
            if(!this.fold){
              this.listScroll = new BScroll(this.$refs.listWrapper, {
                click: true
              });
            }
          return !this.fold;
        }
      }
    },
    components: {
      cartControl
    },
    methods: {
      toggleList(){
        if (!this.totalCount) {
          return;
        }
        this.fold = !this.fold;
      },
      removeAll(){
        this.selectGoods.forEach((food) => {
          food.count = 0;
        });
      },
      hideList(){
          this.fold = true;
      },
      pay(){
          if(this.totalPrice < this.minPrice){
              return;
          }
          window.alert(`支付${this.totalPrice }元`)
      }
    }
  }
</script>

<style scoped lang="less">
  @import "../../common/css/mixin";

  .cart {
    position: fixed;
    left: 0;
    bottom: 0;
    height: 48px;
    width: 100%;
    z-index: 50;
    .content {
      display: flex;
      height: 100%;
      font-size: 0;
      .content-left {
        flex: 1;
        background-color: #141d27;
        .logo-wrapper {
          display: inline-block;
          position: relative;
          top: -10px;
          margin: 0 12px;
          padding: 6px;
          width: 56px;
          height: 56px;
          box-sizing: border-box;
          vertical-align: top;
          border-radius: 50%;
          background-color: #141d27;
          .logo {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: rgb(43, 52, 60);
            text-align: center;
            &.highlight {
              background-color: rgb(0, 160, 226);
            }
            .iconfont {
              font-size: 28px;
              line-height: 44px;
              color: #80858a;
              &.highlight {
                color: #fff;
              }
            }
          }
          .num {
            position: absolute;
            top: 0;
            right: 0;
            width: 24px;
            height: 16px;
            line-height: 16px;
            text-align: center;
            border-radius: 10px;
            font-size: 9px;
            font-weight: 700;
            color: #fff;
            background-color: rgb(240, 20, 20);
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
          }
        }
        .price {
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top: 12px;
          box-sizing: border-box;
          padding-right: 12px;
          border-right: 1px solid rgba(255, 255, 255, .1);
          font-size: 16px;
          font-weight: 700;
          color: rgba(255, 255, 255, .4);
          &.highlight {
            color: #fff;
          }
        }

        .desc {
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top: 12px;
          font-size: 12px;
          margin-left: 12px;
          color: rgba(255, 255, 255, .4);;

        }
      }
      .content-right {
        flex: 0 0 105px;
        background-color: #2b333b;
        .pay {
          color: rgba(255, 255, 255, .4);
          font-weight: 700;
          line-height: 48px;
          font-size: 12px;
          text-align: center;
          &.not-enough {
            background-color: #2b333b;
          }
          &.enough {
            background-color: #00d43c;
            color: #fff;
          }
        }
      }
    }
    .slider-enter-active {
      transition: all 0.3s ease;
    }
    .slider-leave-active {
      transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
    }
    .slider-enter,
    .slider-leave-to {
      transform: translateY(100%);
      opacity: 0;
    }
    .cart-list {
      position: fixed;
      width: 100%;
      bottom: 48px;
      left: 0;
      z-index: -1;
      .list-header {
        width: 100%;
        background-color: #f3f5f7;
        padding: 0 18px;
        box-sizing: border-box;
        height: 40px;
        line-height: 40px;
        .border-1px(rgba(7, 17, 27, .1));
        .title {
          display: inline-block;
          font-size: 14px;
          color: rgb(7, 17, 27);
          font-weight: 200;
        }
        .empty {
          display: inline-block;
          float: right;
          font-size: 12px;
          color: rgb(0, 160, 220);
        }
      }
      .list-content {
        max-height: 217px;
        width: 100%;
        padding: 0 18px 18px 18px;
        box-sizing: border-box;
        background-color: #fff;
        /*overflow: auto;*/
        overflow: hidden;
        .list-box {
          width: 100%;
          .list-food {
            position: relative;
            width: 100%;
            padding: 12px 0;
            box-sizing: border-box;
            .border-1px(rgba(7, 17, 27, .1));
            .name {
              display: inline-block;
              font-size: 14px;
              color: rgb(7, 17, 27);
              line-height: 24px;
            }
            .price {
              position: absolute;
              top: 12px;
              right: 102px;
              height: 24px;
              line-height: 24px;
              display: inline-block;
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
            .cartcontrol-wrapper {
              position: absolute;
              top: 12px;
              right: 18px;
              display: inline-block;
            }
          }
        }

      }
    }
  }
  .list-mark{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index:-2;
    opacity:1;
    background-color: rgba(7,17,27,.6);
    backdrop-filter: blur(10px);
  }
  .fade-enter-active {
    background-color: rgba(7,17,27,.6);
    transition: all 0.3s ease;
  }
  .fade-leave-active {
    transition: all 0.3s ease;
  }
  .fade-enter,
  .fade-leave-to {
    background-color: rgba(7,17,27,0);
    transform: translateY(100%);
    opacity: 0;
  }

</style>
