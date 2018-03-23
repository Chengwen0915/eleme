<template>
    <div class="cart">
      <div class="content">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo">
              <i class="iconfont icon-cart"></i>
            </div>
          </div>
          <div class="price">¥10</div>
          <div class="desc">另需要配送费4元</div>
        </div>
        <div class="content-right">
          ¥20起送
        </div>
      </div>

    </div>
</template>

<script>
    export default {
        data(){
            return {}
        }
    }
</script>

<style scoped lang="less">
  .cart{
    position: fixed;
    left: 0;
    bottom: 0;
    height: 48px;
    width: 100%;
    z-index:50;
    .content{
      display: flex;
      height: 100%;
      font-size: 0;
      .content-left{
        flex: 1;
        background-color: #141d27;
        .logo-wrapper{
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
          .logo{
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background:rgb(43,52,60);
            text-align: center;
            .iconfont{
              font-size: 28px;
              line-height: 44px;
              color: #80858a;
            }
          }
        }
        .price{
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top: 12px;
          box-sizing: border-box;
          padding-right: 12px;
          border-right: 1px solid rgba(255,255,255,.1);
          font-size: 16px;
          font-weight: 700;
          color: rgba(255,255,255,.4);
        }

        .desc{
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top: 12px;
          font-size: 12px;
          margin-left: 12px;
          color: rgba(255,255,255,.4);;

        }
      }
      .content-right{
        flex:0 0 105px;
        color: rgba(255,255,255,.4);
        font-weight: 700;
        line-height: 48px;
        font-size: 12px;
        text-align: center;

        background-color: #2b333b;
      }
    }
  }

</style>
