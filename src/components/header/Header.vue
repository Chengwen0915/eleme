<template>
  <div class="header">
    <div class="content-wrapper">

      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>

      <div class="content">

        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>

        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟到达
        </div>

        <div v-if="seller.supports" class="supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>

        <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="iconfont icon-right-arrow"></i>
        </div>

      </div>
    </div>

    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="iconfont icon-right-arrow"></i>
    </div>

    <div class="bg">
      <img :src="seller.avatar" alt="" width="100%" height="100%">
    </div>

    <transition name='fade'>
      <div v-show="detailShow" class="detail" >
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
                <star :score='seller.score' :size="48"></star>
            </div>

            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>

            <ul v-if='seller.supports' class="supports">
              <li v-for="(item,index) in seller.supports" :key='index' class='support-item'>
              <span class='icon' :class="classMap[item.type]"></span>
              <span class="text">{{item.description}}</span>
              </li>
            </ul>

            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>

            <p class="bulletin">{{seller.bulletin}}</p>
          </div>
        </div>
        <div class="detail-close">
          <i class="iconfont icon-close" @click="closeDetail"></i>
        </div>
      </div>
    </transition>

  </div>
</template>

<script>
import star from "../star/Star";
export default {
  props: ["seller"],
  data() {
    return {
      classMap: [],
      detailShow: false
    };
  },
  created() {
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
  },
  methods: {
    showDetail() {
      this.detailShow = true;
    },
    closeDetail() {
      this.detailShow = false;
    }
  },
  components: {
    star
  }
};
</script>

<style scoped lang="less">
@import "../../common/css/mixin";
.header {
  position: relative;
  color: #fff;
  overflow: hidden;
  background-color: rgba(7, 17, 27, 0.5);
  .content-wrapper {
    position: relative;
    padding: 24px 12px 18px 24px;
    font-size: 0;
    .avatar {
      display: inline-block;
      width: 64px;
      height: 64px;
      vertical-align: top;
      img {
        border-radius: 2px;
      }
    }
    .content {
      display: inline-block;
      font-size: 14px;
      margin-left: 16px;
      .title {
        margin: 2px 0 8px 0;
        .brand {
          display: inline-block;
          margin-top: 2px;
          width: 30px;
          height: 18px;
          .bg-img("header/brand");
          background-size: 30px 18px;
          background-repeat: no-repeat;
          vertical-align: top;
        }
        .name {
          margin-left: 6px;
          font-size: 16px;
          font-weight: bold;
        }
      }
      .description {
        line-height: 12px;
        font-size: 12px;
        font-weight: 200;
        margin-bottom: 5px;
        color: #fff;
      }
      .supports {
        .icon {
          display: inline-block;
          vertical-align: middle;
          width: 12px;
          height: 12px;
          margin-right: 4px;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          &.decrease {
            .bg-img("header/decrease_1");
          }
          &.discount {
            .bg-img("header/discount_1");
          }
          &.guarantee {
            .bg-img("header/guarantee_1");
          }
          &.invoice {
            .bg-img("header/invoice_1");
          }
          &.special {
            .bg-img("header/special_1");
          }
        }
        .text {
          line-height: 12px;
          font-size: 12px;
          color: rgb(255, 255, 255);
          font-weight: 200;
        }
      }
      .support-count {
        position: absolute;
        right: 12px;
        bottom: 18px;
        padding: 0 8px;
        height: 24px;
        line-height: 24px;
        border-radius: 14px;
        background-color: rgba(0, 0, 0, 0.2);
        color: rgb(255, 255, 255);
        .count {
          display: inline-block;
          vertical-align: top;
          font-size: 10px;
          margin-right: 2px;
        }
        .icon-right-arrow {
          font-size: 10px;
          line-height: 24px;
        }
      }
    }
  }
  .bulletin-wrapper {
    position: relative;
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background-color: rgba(7, 17, 27, 0.2);
    .bulletin-title {
      display: inline-block;
      vertical-align: top;
      margin-top: 10px;
      width: 22px;
      height: 12px;
      .bg-img("header/bulletin");
      background-size: 22px 12px;
      background-repeat: no-repeat;
    }
    .bulletin-text {
      font-size: 10px;
      color: rgb(255, 255, 255);
      font-weight: 200;
      margin: 0 4px;
    }
    .icon-right-arrow {
      position: absolute;
      right: 12px;
      top: 3px;
      font-size: 10px;
      line-height: 24px;
    }
  }
  .bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }
  .fade-enter-active {
    transition: all 0.3s ease;
  }
  .fade-leave-active {
    transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
  }
  .fade-enter,
  .fade-leave-to {
    transform: translateY(-100%);
    opacity: 0;
  }
  .detail {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(7, 17, 27, 0.8);
    .detail-wrapper {
      width: 100%;
      min-height: 100%;
      .detail-main {
        margin-top: 64px;
        padding-bottom: 64px;
        .name {
          font-weight: 700;
          font-size: 16px;
          text-align: center;
          line-height: 16px;
        }
        .star-wrapper {
          margin-top: 18px;
          padding: 2px 0;
          text-align: center;
        }
        .title {
          display: flex;
          width: 80%;
          margin: 28px auto 24px auto;
          .line {
            flex: 1;
            position: relative;
            top: -8px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
          }
          .text {
            padding: 0 12px;
            font-size: 14px;
            font-weight: 700;
          }
        }
        .supports {
          width: 80%;
          margin: 0 auto;
          .support-item {
            padding: 0 12px;
            margin-bottom: 12px;
            // font-size: 0;
            &:last-child {
              margin-bottom: 0;
            }
            .icon {
              display: inline-block;
              width: 16px;
              height: 16px;
              vertical-align: top;
              margin-right: 6px;
              background-size: 16px 16px;
              background-repeat: no-repeat;
              &.decrease {
                .bg-img("header/decrease_2");
              }
              &.discount {
                .bg-img("header/discount_2");
              }
              &.guarantee {
                .bg-img("header/guarantee_2");
              }
              &.invoice {
                .bg-img("header/invoice_2");
              }
              &.special {
                .bg-img("header/special_2");
              }
            }
            .text {
              line-height: 12px;
              font-size: 12px;
            }
          }
        }
        .bulletin {
          width: 80%;
          margin: 0 auto;
          font-size: 12px;
          line-height: 24px;
          color: rgb(255, 255, 255);
        }
      }
    }
    .detail-close {
      position: relative;
      width: 32px;
      height: 32px;
      line-height: 32px;
      margin: -64px auto 0 auto;
      clear: both;
      font-size: 32px;
      .icon-close {
        font-size: 32px;
      }
    }
  }
}
</style>
