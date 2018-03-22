<template>
  <div class="goods">

    <div class="menu-wrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>

    </div>

    <div class="foods-wrapper">
      <ul>
        <li v-for="(item,index) in goods" class="food-list" :key="index">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food,index1) in item.foods" class="food-item border-1px">
              <div class="icon">
                <img :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="description">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span>¥{{food.price}}</span>
                  <span v-show="food.oldPrice">¥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
  export default {
      props:["seller"],
    data(){
      return {
          goods:{},
          classMap: []
      }
    },
    created(){
      this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
      this.axios.get('/api/goods').then(res => {
        this.goods = res.data.data;
        console.log(res.data.data)
      }).catch(res => {
        console.log(res);
      });

      this.$router.push('goods'); // 页面加载时跳转
    }
  }
</script>

<style scoped lang='less'>
  @import "../../common/css/mixin";
  .goods {
    display: flex;
    position: absolute;
    top: 177px;
    bottom: 46px;
    left: 0;
    width: 100%;
    /*overflow: hidden;*/
    .menu-wrapper {
      flex: 0 0 80px;
      width: 80px;
      background-color: #f3f5f7;
      .menu-item{
        display: table;
        width: 56px;
        height: 54px;
        line-height: 14px;
        padding:0 12px;
        .icon {
          display: inline-block;
          width: 12px;
          height: 12px;
          margin-right: 2px;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          &.decrease {
            .bg-img("goods/decrease_3");
          }
          &.discount {
            .bg-img("goods/discount_3");
          }
          &.guarantee {
            .bg-img("goods/guarantee_3");
          }
          &.invoice {
            .bg-img("goods/invoice_3");
          }
          &.special {
            .bg-img("goods/special_3");
          }
        }
        .text{
          display: table-cell;
          width: 56px;
          vertical-align: middle;
          font-size: 12px;
          text-align: center;
          .border-1px(rgba(7,17,27,.1));
        }
      }

    }
    .foods-wrapper {
      flex: 1;
      .food-list{
        .title{
          position: relative;
          height: 26px;
          font-size: 12px;
          line-height: 26px;
          padding-left: 14px;
          color: rgb(147,153,159);
          background-color: #f3f5f7;
          &:before{
            position: absolute;
            top: 0;
            left: 0;
            width: 2px;
            height: 100%;
            background-color:#d9dde1;
            content: '';
          }
        }
        .food-item{
          display: flex;
          padding:18px;
          .border-1px(rgba(7,17,27,.1));
          &:last-of-type{
            .border-none();
          }
          .icon{
            flex: 0 0 57px;
            img{
              width: 57px;
              height: 57px;
              border-radius: 4px;
            }
          }
          .content{
            flex:1;
            vertical-align: top;
            padding-left: 10px;
            .name{
              position: relative;
              top: 2px;
              line-height: 14px;
              color: rgb(7,17,27);
              font-size: 14px;
              vertical-align: top;
            }
            .description{
              font-size: 10px;
              color: rgb(147,153,159);
              line-height: 10px;
              margin: 8px 0;
            }
            .extra{
              font-size: 10px;
              color: rgb(147,153,159);
              line-height: 10px;
              margin-bottom: 8px;
              span:first-of-type{
                margin-right: 12px;
              }
            }
          }
        }
      }


    }

  }


</style>
