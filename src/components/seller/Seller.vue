<template>
  <div class="seller-wrapper" ref="sellers">
    <div class="seller">
      <div class="seller-info">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star class="star" :size="36" :score="seller.score"></star>
          <span class="info">({{seller.ratingCount}})</span>
          <span class="info">月售{{seller.sellCount}}单</span>
        </div>

        <ul class="remark">
          <li class="block">
            <h2 class="name">起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2 class="name">商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2 class="name">平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>分钟
            </div>
          </li>
        </ul>


        <div class="collect">
          <span @click="toggloLike" class="iconfont icon-heart" :class="{'active': collect}"></span>
          <span class="text">{{collectText}}</span>
        </div>


      </div>
      <split></split>
      <div class="bulletin ">
        <h1 class="title">公告与活动</h1>
        <div class="text border-1px">{{seller.bulletin}}</div>
      </div>

      <ul class="list">
        <li v-for="item in seller.supports" class="item">
          <span class="icon" :class="classMap[item.type]"></span>
          {{item.description}}
        </li>
      </ul>
      <split></split>

      <div class="img-box">
        <h1 class="title">商家实景</h1>
        <div class="list-wrapper" ref="picWrapper">
          <ul ref="picList">
            <li v-for="img in seller.pics" class="img-item">
              <img :src="img" alt="" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>

      <split></split>

      <div class="shops-info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li v-for="info in seller.infos" class="detail-info border-1px">
            {{info}}
          </li>
        </ul>
      </div>


    </div>
    <cart :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></cart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import split from '../split/Split.vue'
  import star from '../star/Star.vue'
  import cart from '../cart/Cart.vue'

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        collect: false,
        classMap: ["decrease", "discount", "special", "invoice", "guarantee"]
      }
    },
    components: {
      split,
      star,
      cart
    },
    computed: {
      collectText(){
        return this.collect ? '已收藏' : '收藏';
      }
    },
    created(){
    },

    mounted (){
      this.scroll = new BScroll(this.$refs.sellers, {
        click: true
      });

    },
    watch: {
      'seller'(){
        this._initPics()
      }
    },
    methods: {
      toggloLike(){
        this.collect = this.collect ? false : true;
      },
      _initPics()
      {
        if (this.seller.pics) {
          let picW = 120;
          let margin = 6;
          let width = (picW + margin) * this.seller.pics.length - margin;
          this.$refs.picList.style.width = width + 'px';
          this.$nextTick(() => {
            this.picScroll = new BScroll(this.$refs.picWrapper, {
              click: true,
              scrollX: true,
              eventPassthrough: "vertical"
            })
          })
        }
      }
    }
  }
</script>

<style scoped lang='less'>
  @import '../../common/css/mixin';

  .seller-wrapper {
    position: absolute;
    top: 177px;
    bottom: 48px;
    left: 0;
    width: 100%;
    overflow: hidden;
    box-sizing: border-box;
    .seller-info {
      position: relative;
      width: 100%;
      padding: 18px;
      box-sizing: border-box;
      .title {
        font-size: 14px;
        color: rgb(7, 17, 27);
        line-height: 14px;
        margin-bottom: 8px;
        font-weight: 700;
      }
      .desc {
        padding-bottom: 18px;
        .border-1px(rgba(7, 17, 27, .1));
        .star {
          display: inline-block;
        }
        .info {
          position: relative;
          top: -2px;
          vertical-align: top;
          font-size: 10px;
          color: rgb(77, 85, 93);
          line-height: 18px;
        }
        .info:first-of-type {
          margin-right: 8px;
        }
        .info:last-of-type {
          margin-right: 12px;
        }
      }
      .remark {
        display: flex;
        .block {
          flex: 1;
          margin-top: 18px;
          text-align: center;
          .name {
            font-size: 10px;
            color: rgb(147, 153, 159);
            line-height: 10px;
            margin-bottom: 4px;
          }
          .content {
            font-size: 10px;
            font-weight: 200;
            color: rgb(7, 17, 27);
            line-height: 24px;
            .stress {
              font-size: 24px;
            }
          }
        }
        .block:nth-of-type(2) {
          border-right: 1px solid rgba(7, 17, 27, .1);
          border-left: 1px solid rgba(7, 17, 27, .1);
        }
      }
      .collect {
        position: absolute;
        right: 11px;
        top: 18px;
        width: 50px;
        text-align: center;
        .iconfont {
          display: block;
          font-size: 24px;
          color: rgb(147, 153, 159);
          line-height: 24px;

          margin-bottom: 4px;
          &.active {
            color: rgb(240, 20, 20);
          }
        }
        .text {
          font-size: 10px;
          color: rgb(77, 85, 93);
          line-height: 20px;
        }
      }
    }
    .bulletin {
      padding: 18px 18px 0 18px;
      .title {
        font-size: 14px;
        color: rgb(7, 17, 27);
        line-height: 14px;
        margin-bottom: 8px;
        font-weight: 700;
      }
      .text {
        padding: 8px 12px 18px 12px;
        font-size: 12px;
        font-weight: 200;
        color: rgb(240, 20, 20);
        line-height: 24px;
        .border-1px(rgba(7, 17, 27, .1));

      }
    }
    .list {
      padding: 0 16px;
      .item {
        padding: 16px 0 16px 12px;
        font-size: 12px;
        font-weight: 200;
        color: rgb(7, 17, 27);
        line-height: 16px;
        .border-1px(rgba(7, 17, 27, .1));
        .icon {
          display: inline-block;
          width: 16px;
          height: 16px;
          vertical-align: top;
          background-size: 16px 16px;
          &.decrease {
            .bg-img("seller/decrease_4");
          }
          &.discount {
            .bg-img("seller/discount_4");
          }
          &.guarantee {
            .bg-img("seller/guarantee_4");
          }
          &.invoice {
            .bg-img("seller/invoice_4");
          }
          &.special {
            .bg-img("seller/special_4");
          }
        }
      }
      .item:last-of-type {
        .border-none();
      }
    }
    .img-box {
      width: 100%;
      padding: 18px;
      box-sizing: border-box;
      .title {
        font-size: 14px;
        color: rgb(7, 17, 27);
        line-height: 14px;
        margin-bottom: 8px;
        font-weight: 700;
      }
      .list-wrapper {
        width: 100%;
        overflow: hidden;
        ul {
          overflow: hidden;
          font-size: 0;
          .img-item {
            display: inline-block;
            width: 120px;
            height: 90px;
            margin-right: 6px;
          }
          .img-item:last-of-type {
            margin-right: 0;
          }
        }
      }

    }
    .shops-info {
      padding: 18px 18px 0 18px;
      .title {
        font-size: 14px;
        color: rgb(7, 17, 27);
        line-height: 14px;
        padding-bottom: 12px;
        font-weight: 700;
        .border-1px(rgba(7, 17, 27, .1));
      }
      .detail-info {
        padding: 16px 0 16px 12px;
        font-size: 12px;
        font-weight: 200;
        color: rgb(7, 17, 27);
        line-height: 16px;
        .border-1px(rgba(7, 17, 27, .1));
      }
    }
  }

</style>
