<template>
  <transition name="fade">
    <div class="food" v-show="showFood" ref="food">
      <div class="food-content">
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
            <CartControl :food="food"></CartControl>
          </div>

          <input @click.stop.prevent="addFirst" type="button" v-show="!food.count || food.count === 0" class="btn"
                 value="加入购物车">

        </div>

        <split v-show="food.info"></split>

        <div class="sellerDesc" v-show="food.info">
          <h2 class="text">商家介绍</h2>
          <div class="desc">{{food.info}}</div>
        </div>

        <split></split>

        <div class="rating">
          <h2 class="text">商品评价</h2>
          <ratingSelect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings" @type="type" @content="content"></ratingSelect>
          <!--<ratingSelect :desc="desc" :ratings="food.ratings"></ratingSelect>-->
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="(rating,index) in food.ratings" :key="index" class="rating-item border-1px">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" :src="rating.avatar" alt="" width="12" height="12">
                </div>

                <div class="time">{{rating.rateTime | formatterTime}}</div>
                <div class="text-desc">
                  <span class="iconfont"
                        :class="{'icon-good':rating.rateType === 0,'icon-bad':rating.rateType === 1}"></span>
                  <span class="desc">{{rating.text}}</span>
                </div>
              </li>
            </ul>

            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import CartControl from '../cartControl/CartControl'
  import split from '../split/Split'
  import ratingSelect from '../ratingSelect/RatingSelect'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    data(){
      return {
        showFood: false,
        selectType: 2,
        onlyContent: false,
        desc: {
          all: "全部",
          positive: "推荐",
          negative: "吐槽"
        }
      }
    },

    components: {
      CartControl,
      split,
      ratingSelect
    },
    filters:{
      formatterTime(input){
        let time = new Date(input);
        let year = time.getFullYear();
        let mon = time.getMonth() + 1;
        let day = time.getDate();
        let h = time.getHours();
        let m = time.getMinutes();
        let s = time.getSeconds();
        let str = `${year}-${mon}-${day} ${h}:${m}:${s}`;
        return str;
      }
    },
    methods: {
      show(){
        this.showFood = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        })
      },
      goBack(){
        this.showFood = false;
      },
      addFirst(){
        if (!this.food.count) {
          Vue.set(this.food, "count", 1)
        }
      },
      needShow(type,text){
          if(this.onlyContent && !text){
              return false;
          }

          if(this.selectType === 2){
              return true;
          }else{
              return type === this.selectType;
          }
      },
      type(type){
          this.selectType = type;
          this.$nextTick( () => {
              this.scroll.refresh()
          })
      },
      content(content){
        this.onlyContent = content;
        this.$nextTick( () => {
          this.scroll.refresh()
        })
      },
    }
  }
</script>

<style scoped lang="less">
  @import "../../common/css/mixin";

  .food {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 48px;
    width: 100%;
    z-index: 30;
    background-color: #fff;
    overflow: hidden;
    &.fade-enter-active, &.fade-leave-active {
      transition: all 0.2s ease;
    }
    &.fade-enter,
    &.fade-leave-to {
      transform: translateX(100%);
      opacity: 0;
    }
    .food-content {
      width: 100%;
      height: auto;

      .img-box {
        position: relative;
        width: 100%;
        img {
          width: 100%;
          height: auto;
        }
        .iconfont {
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
        .price-info {
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
          .old-price {
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
        .cart-control {
          position: absolute;
          right: 18px;
          bottom: 18px;
        }
        .btn {
          position: absolute;
          right: 18px;
          bottom: 18px;
          width: 74px;
          height: 24px;
          border: none;
          outline: none;
          border-radius: 12px;
          font-size: 10px;
          line-height: 12px;
          background-color: rgb(0, 160, 220);
          color: #fff;
        }

      }

      .sellerDesc {
        position: relative;
        width: 100%;
        padding: 18px;
        box-sizing: border-box;
        .text {
          font-weight: 200;
          line-height: 14px;
          font-size: 14px;
          color: rgb(7, 17, 27);
          margin-bottom: 6px;
        }
        .desc {
          font-weight: 200;
          color: rgb(77, 85, 93);
          line-height: 24px;
          font-size: 12px;
        }
      }
      .rating {
        padding: 18px 0;
        .text {
          margin-left: 18px;
          font-weight: 200;
          line-height: 14px;
          font-size: 14px;
          color: rgb(7, 17, 27);
        }
        .rating-wrapper {
          position: relative;
          padding: 0 18px;
          .rating-item {
            padding: 16px 0;
            .border-1px(rgba(7, 17, 27, .1));
            .user {
              position: absolute;
              right: 0;
              top: 16px;
              .name {
                margin-right: 6px;
                font-size: 10px;
                color: rgb(147, 153, 159);
                line-height: 12px;
              }
              .avatar{
                border-radius: 50%;
              }
            }
            .time {
              font-size: 10px;
              color: rgb(147, 153, 159);
              line-height: 12px;
              margin-bottom: 6px;
            }
            .text-desc {
              .icon-good, .icon-bad {
                font-size: 12px;
                line-height: 24px;
                margin-left: 4px;
              }
              .icon-good {
                color: rgb(0, 160, 220);
              }
              .icon-bad {
                color: rgb(147, 153, 159);
              }
              .desc {
                font-size: 12px;
                color: rgb(7, 17, 27);
                line-height: 16px;
              }
            }
          }
          .no-rating{
            padding: 16px 0;
            font-size: 12px;
            line-height: 12px;
            color: rgb(147,153,159);
          }

        }
      }
    }
  }


</style>
