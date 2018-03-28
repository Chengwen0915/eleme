<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <router-link class="tab-item" to="/goods">商品</router-link>
      <router-link class="tab-item" to="/ratings">评价</router-link>
      <router-link class="tab-item" to="/seller">商家</router-link>

    </div>
    <div class="content">
      <keep-alive><router-view :seller="seller" ></router-view></keep-alive>

    </div>
  </div>


</template>

<script>
  import header from "./components/header/Header";
  export default {
    name: "App",
    data(){
      return {
        seller: {}
      }
    },
    components: {
      "v-header": header
    },
    created(){
      this.axios.get('/api/seller').then(res => {
        this.seller = res.data.data;
        //let obj = this.urlParse();
        //this.seller = Object.assign({},obj,res.data.data);

      }).catch(res => {
        console.log(res);
      });

    },
    methods: {
      urlParse(){
        //?id=123456&a=b
          let url = window.location.search;
          let obj = {};
          let reg = /[?&][^?&]+=[^?&]+/g;
          let arr = url.match(reg);
          if(arr){
              arr.forEach( (item) => {
                  let tempArr = item.substring(1).split("=");
                  let key = decodeURIComponent(tempArr[0]);
                  let val = decodeURIComponent(tempArr[1]);
                  obj[key] = val;
              })
          }
          return obj;
      }
    }
  };
</script>

<style scope lang='less'>
  @import './common/css/mixin.less';

  #app {
    .tab {
      display: flex;
      width: 100%;
      height: 40px;
      line-height: 40px;
      .border-1px(rgba(7, 17, 27, 0.1));

      .tab-item {
        flex: 1;
        text-align: center;
        font-size: 14px;
        color: rgb(77, 85, 93);
      }
      .router-link-active {
        color: rgb(240, 20, 20);
      }
    }
  }
</style>
