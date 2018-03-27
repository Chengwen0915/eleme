<template>
  <div class="rating-select">
    <div class="rating-type border-1px">
      <span @click="select(2)" class="block positive" :class="{'active':selectType === 2}">{{desc.all}} <span
        class="count">{{ratings.length}}</span></span>
      <span @click="select(0)" class="block positive" :class="{'active':selectType === 0}">{{desc.positive}} <span
        class="count">{{positive.length}}</span></span>
      <span @click="select(1)" class="block negative" :class="{'active':selectType === 1}">{{desc.negative}} <span
        class="count">{{negative.length}}</span></span>
    </div>
    <div @click="toggleContent" class="switch" :class="{'on':onlyContent == true}">
      <span class="iconfont icon-right-f"></span>
      <span class="text">只看内容评价</span>
    </div>

  </div>
</template>

<script>

  export default {
    props: {
      ratings: {
        type: Array,
        default(){
          return [];
        }
      },
      selectType: {
        type: Number,
        default: 2
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default(){
          return {
            all: "全部",
            positive: "满意",
            negative: "不满意",
          }
        }
      }
    },
    data(){
      return {
        type: 1,
        only: false
      }
    },
    computed: {
      positive(){
        let arr = [];
        for (let i = 0; i < this.ratings.length; i++) {
          if (this.ratings[i].rateType === 0) {
            arr.push(this.ratings[i])
          }
        }
        return arr;
      },
      negative(){
        let arr = [];
        for (let i = 0; i < this.ratings.length; i++) {
          if (this.ratings[i].rateType === 1) {
            arr.push(this.ratings[i])
          }
        }
        return arr;
      }
    },
    methods: {
      select(type){
        this.type = type;
        this.$emit('type', type);
      },
      toggleContent(){
        this.only = !this.only;
        this.$emit('content', this.only);
      }
    }
  }
</script>

<style scoped lang="less">
  @import '../../common/css/mixin';

  .rating-select {
    .rating-type {
      padding: 18px 0;
      margin: 0 18px;
      .border-1px(rgba(7, 17, 27, .1));
      font-size: 0;
      .block {
        display: inline-block;
        padding: 8px 12px;
        font-size: 12px;
        border-radius: 1px;
        color: rgb(77, 85, 93);
        line-height: 16px;
        margin-right: 8px;
        &.active {
          color: #fff;
        }
        &.positive {
          background-color: rgba(0, 160, 220, .2);
          &.active {
            background-color: rgb(0, 160, 220);
          }
        }
        &.negative {
          background-color: rgba(77, 85, 93, .2);
          &.active {
            background-color: rgb(77, 85, 93);
          }
        }
        .count {
          font-size: 8px;
          margin-left: 4px;
        }
      }
    }
    .switch {
      padding: 12px 18px;
      line-height: 24px;
      font-size: 12px;
      color: rgb(147, 153, 159);
      border-bottom: 1px solid rgba(7, 17, 27, .1);
      &.on {
        .iconfont {
          color: #00c850;
        }
      }
      .iconfont {
        font-size: 24px;
        line-height: 24px;
        color: rgb(147, 153, 159);
        vertical-align: middle;
        margin-right: 4px;

      }
      .text {
        font-size: 12px;
        line-height: 24px;
        color: rgb(147, 153, 159);
        vertical-align: middle
      }
    }
  }

</style>
