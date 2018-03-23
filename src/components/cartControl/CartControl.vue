<template>
    <div class="cartControl">
      <!--<transition name="move">-->
        <div class="cart-decrease iconfont icon-reduce" v-show="food.count > 0" @click='reduceCart'></div>
      <!--</transition>-->
      <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>

      <div class="cart-add iconfont icon-add" @click="addCart"></div>
    </div>
</template>

<script>
  import Vue from 'vue';
    export default {
        props:["food"],
        data(){
            return {

            }
        },
      created(){
      },
      methods:{
        addCart(){
            if(!this.food.count){
                Vue.set(this.food,'count',1);
            }else{
              this.food.count++;
            }
          console.log("click");

        },
        reduceCart(){
            if(this.food.count){
              this.food.count--;
            }

        }
      }
    }
</script>

<style scoped lang="less">
  .cartControl{
    font-size: 0;
    .cart-decrease{
      display: inline-block;
      color: #0096ce;
      font-size: 24px;
      vertical-align: top;
     /* transition: all 0.02s linear;
      &.move-transition{
        opacity:1;
        transform: translate3d(0 0 0) ;
      }
      &.move-enter,.move-leave{
        opacity: 0;
        transform: translate3d(0 0 0);
      }*/
    }
    .cart-count{
      display: inline-block;
      width: 24px;
      text-align: center;
      font-size: 10px;
      line-height: 24px;
      color: rgb(143,153,159);
      height: 100%;vertical-align: middle;
    }
    .cart-add{
      display: inline-block;
      color: #0096ce;
      font-size: 24px;vertical-align: top;
    }
  }

</style>
