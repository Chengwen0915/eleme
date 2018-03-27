<template>
  <div class="ratings-wrapper" ref="ratings">
    <div class="ratings">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">

          <div class="service-wrapper">
            <span class="title">服务态度</span>
            <star class="star" :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="service-wrapper">
            <span class="title">商品评分</span>
            <star class="star" :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="time">
            <span class="title">送达时间</span>
            <span class="deliveryTime">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingSelect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="ratings"
                    @type="type" @content="content"></ratingSelect>
      <div class="ratings-content">
        <ul>
          <li v-for="(rating,index) in ratings" :key="index" class="rating-item"  v-show="needShow(rating.rateType,rating.text)">
            <div class="avatar">
              <img :src="rating.avatar" width="28" height=""/>
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star  class="star" :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="iconfont icon-good"></span>
                <span class="item" v-for="(item,index1) in rating.recommend" :key="index1">{{item}}</span>
              </div>
              <div class="time">{{rating.rateTime | formatterTime}}</div>
            </div>
          </li>
        </ul>
      </div>

    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import star from '../star/Star'
  import split from '../split/Split'
  import ratingSelect from '../ratingSelect/RatingSelect'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        ratings: [],
        selectType: 2,
        onlyContent: false,
        desc: {
          all: "全部",
          positive: "满意",
          negative: "不满意"
        }
      }
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
    components: {
      star,
      split,
      ratingSelect
    },
    created(){
      this.axios.get('/api/ratings').then(res => {
        this.ratings = res.data.data;
        console.log(this.ratings);
        this.$nextTick( () => {
          this.scroll = new BScroll(this.$refs.ratings,{
            click:true
          });
        });
        console.log(res.data.data)
      }).catch(res => {
        console.log(res);
      });
    },
    methods: {
      type(type){
        this.selectType = type;
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      content(content){
        this.onlyContent = content;
        this.$nextTick(() => {
          this.scroll.refresh()
        })
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
    }
  }
</script>

<style scoped lang='less'>
  @import "../../common/css/mixin";
  .ratings-wrapper {
    position: absolute;
    top: 177px;
    bottom: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
    .overview {
      display: flex;
      padding: 18px 0;
      .overview-left {
        flex: 0 0 137px;
        width: 137px;
        padding: 6px 0;
        border-right: 1px solid rgba(7, 17, 27, .1);
        text-align: center;
        @media only screen and(max-width: 320px) {
          flex: 0 0 120px;
          width: 120px;
        }
        .score {
          font-size: 24px;
          color: rgb(255, 153, 0);
          line-height: 28px;
        }
        .title {
          margin: 6px 0 8px 0;
          font-size: 12px;
          color: rgb(7, 17, 27);
          line-height: 12px;
        }
        .rank {
          font-size: 10px;
          color: #93999f;
        }
      }
      .overview-right {
        flex: 1;
        padding: 6px 0 6px 24px;
        @media only screen and(max-width: 320px) {
          padding: 6px 0 6px 6px;
        }
        .service-wrapper {
          line-height: 18px;
          margin-bottom: 8px;
          font-size: 0;
          .title, .star, .score {
            display: inline-block;
            font-size: 12px;
            line-height: 18px;
          }
          .title {
            color: rgb(7, 17, 27);
          }
          .star {
            vertical-align: top;
            margin: 0 12px;
          }
          .score {
            color: rgb(255, 153, 0);
          }
        }
        .time {
          font-size: 0;
          .title {
            display: inline-block;
            font-size: 12px;
            line-height: 18px;
            color: rgb(7, 17, 27);
          }
          .deliveryTime {
            margin-left: 12px;
            font-size: 12px;
            color: rgb(147, 153, 159);
          }
        }
      }
    }
    .ratings-content{
      padding: 0 18px;
      .rating-item{
        display: flex;
        padding: 18px 0;
        .border-1px(rgba(7,17,27,.1));
        .avatar{
          flex:0 0 28px;
          margin-right: 12px;
          width: 28px;
          img{
            border-radius: 50%;
          }
        }
        .content{
          position: relative;
          flex:1;
          .name{
            line-height: 12px;
            margin-bottom: 4px;
            font-size: 10px;
            color: rgb(7,17,27);
          }
          .star-wrapper{
            margin-bottom: 6px;
            font-size: 0;
            .star{
              display: inline-block;
              vertical-align: top;
              margin-right: 6px;
            }
            .delivery{
              display: inline-block;
              vertical-align: top;
              font-size: 10px;
              line-height: 12px;
              font-weight: 200;
              color: rgb(147,153,159);
            }
          }
          .text{
            line-height: 18px;
            color: rgb(7,17,27);
            font-size: 12px;
            margin-bottom: 8px;
          }
          .recommend{
            font-size: 0;
            line-height: 16px;
            .iconfont,.item{
              display: inline-block;
              margin: 0 8px 4px 0;
              font-size: 9px;
            }
            .iconfont{
              color: rgb(0,160,220);
            }
            .item{
              padding: 0 6px;
              border: 1px solid rgba(7,17,27,.1);
              border-radius: 1px;
              color: rgb(147,153,159);
              background-color: #fff;
            }
          }
          .time{
            position: absolute;
            top: 0;
            right: 0;
            line-height: 12px;
            font-size: 10px;
            color: rgb(147,153,159);
          }
        }
      }
    }
  }
</style>
