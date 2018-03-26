<template>
  <div class="goods">

    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}"
            @click="selectMenu(index)">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>

    </div>

    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="food-list food-list-hook" :key="index">
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
                  <span>¥<span class="new-price">{{food.price}}</span></span>
                  <span v-show="food.oldPrice" class="old-price">¥{{food.oldPrice}}</span>
                </div>

                <div class="cart-wrapper">
                  <cartControl :food="food"></cartControl>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <cart :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :select-goods='selectGoods'></cart>

  </div>
</template>

<script>
  import Bscroll from 'better-scroll';
  import cart from '../cart/cart'
  import cartControl from '../CartControl/CartControl'
  export default {
    props: ["seller"],
    data(){
      return {
        goods: {},
        classMap: [],
        count: 0,
        listHeight: [],
        scrollY: 0
      }
    },
    components: {
      cart,
      cartControl
    },
    created(){
      this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
      this.axios.get('/api/goods').then(res => {
        this.goods = res.data.data;
        this.$nextTick(() => {
          this._initScroll();
          this._calcLastHeight();
        });
        console.log(res.data.data)
      }).catch(res => {
        console.log(res);
      });

      this.$router.push('goods'); // 页面加载时跳转
    },
    computed: {
      currentIndex(){
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectGoods(){
        let foods = [];
        /* this.goods.forEach((good) => {
         good.foods.forEach((food) => {
         if (food.count) {
         foods.push(food);
         }
         })
         });*/
        for (let i = 0; i < this.goods.length; i++) {
          for (let j = 0; j < this.goods[i].foods.length; j++) {
            if (this.goods[i].foods[j].count) {
              foods.push(this.goods[i].foods[j]);
            }
          }
        }
        return foods;
      }
    },
    methods: {
      _initScroll(){
        this.menuScroll = new Bscroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new Bscroll(this.$refs.foodsWrapper, {
          probeType: 3,
          click: true
        });
        this.foodsScroll.on("scroll", (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        });
      },
      _calcLastHeight(){
        // 计算右侧每个栏目的距离顶部的高度，放进数组。
        let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook");
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index){
        // 获取所有菜单项
        let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook");
        //获取点击的dom
        let el = foodList[index];
        // 右侧滚到对应对应的位置
        this.foodsScroll.scrollToElement(el, 300);
      }
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
    overflow: hidden;
    .menu-wrapper {
      flex: 0 0 80px;
      width: 80px;
      background-color: #f3f5f7;
      .menu-item {
        display: table;
        width: 56px;
        height: 54px;
        line-height: 14px;
        padding: 0 12px;
        &.current {
          position: relative;
          margin-top: -1px;
          z-index: 10;
          background-color: #fff;
          font-weight: 700;
          .text {
            .border-none();
          }
        }
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
        .text {
          display: table-cell;
          width: 56px;
          vertical-align: middle;
          font-size: 12px;
          text-align: center;
          .border-1px(rgba(7, 17, 27, .1));
        }
      }

    }
    .foods-wrapper {
      flex: 1;
      .food-list {
        .title {
          position: relative;
          height: 26px;
          font-size: 12px;
          line-height: 26px;
          padding-left: 14px;
          color: rgb(147, 153, 159);
          background-color: #f3f5f7;
          &:before {
            position: absolute;
            top: 0;
            left: 0;
            width: 2px;
            height: 100%;
            background-color: #d9dde1;
            content: '';
          }
        }
        .food-item {
          display: flex;
          padding: 18px;
          .border-1px(rgba(7, 17, 27, .1));
          &:last-of-type {
            .border-none();
          }
          .icon {
            flex: 0 0 57px;
            img {
              width: 57px;
              height: 57px;
              border-radius: 4px;
            }
          }
          .content {
            position: relative;
            flex: 1;
            vertical-align: top;
            padding-left: 10px;
            .name {
              position: relative;
              top: 2px;
              line-height: 14px;
              color: rgb(7, 17, 27);
              font-size: 14px;
              vertical-align: top;
            }
            .description, .extra {
              font-size: 10px;
              color: rgb(147, 153, 159);
              line-height: 10px;
            }
            .description {
              margin: 8px 0;
            }
            .extra {
              margin-bottom: 8px;
              span:first-of-type {
                margin-right: 12px;
              }
            }
            .price {
              span:first-of-type {
                color: rgb(240, 20, 20);
                font-size: 10px;
                font-weight: normal;
                line-height: 24px;
                .new-price {
                  font-size: 14px;
                  font-weight: 700;
                  line-height: 24px;
                  color: rgb(240, 20, 20);
                }
              }

              .old-price {
                font-size: 10px;
                font-weight: 700;
                line-height: 24px;
                text-decoration: line-through;
                margin-left: 8px;
                color: rgb(143, 153, 159);
              }
            }
            .cart-wrapper {
              position: absolute;
              right: 0;
              bottom: 0;
            }
          }
        }
      }
    }
  }


</style>
