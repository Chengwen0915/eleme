
<template>
  <div class="star" :class='starType'>
    <span v-for='(item,index) in itemArr' :class='item' class="star-item" :key="index"></span>
  </div>
</template>

<script>
const length = 5;
const cls_on = "on";
const cls_half = "half";
const cls_off = "off";
export default {
  props: ["size", "score"],
  data() {
    return {};
  },
  data(){
    return {

    }
  },
  computed: {
    starType() {
      return 'star-'+this.size;
    },
    itemArr() {
      let result = [];
      let score = Math.floor(this.score * 2) / 2;
      let hasDecimal = score % 1 !== 0;
      let integer = Math.floor(score);
      for (let i = 0; i < integer; i++) {
        result.push(cls_on);
      }
      if (hasDecimal) {
        result.push(cls_half);
      }
      while (result.length < length) {
        result.push(cls_off);
      }
      return result;
    }
  }
};
</script>

<style scoped lang='less'>
@import "../../common/css/mixin.less";
.star {
  font-size: 0;
  .star-item {
    display: inline-block;
    background-repeat: no-repeat;
  }
  &.star-48 {
    .star-item {
      width: 20px;
      height: 20px;
      margin-right: 22px;
      background-size: 20px 20px;
      &:last-child {
        margin-right: 0;
      }
      &.on {
        .bg-img("star/star48_on");
      }
      &.half {
        .bg-img("star/star48_half");
      }
      &.off {
        .bg-img("star/star48_off");
      }
    }
  }
  &.star-36 {
    .star-item {
      width: 15px;
      height: 15px;
      margin-right: 5px;
      background-size: 15px 15px;
      &:last-child {
        margin-right: 0;
      }
      &.on {
        .bg-img("star/star36_on");
      }
      &.half {
        .bg-img("star/star36_half");
      }
      &.off {
        .bg-img("star/star36_off");
      }
    }
  }
  &.star-24 {
    .star-item {
      width: 10px;
      height: 10px;
      margin-right: 3px;
      background-size: 10px 10px;
      &:last-child {
        margin-right: 0;
      }
      &.on {
        .bg-img("star/star24_on");
      }
      &.half {
        .bg-img("star/star24_half");
      }
      &.off {
        .bg-img("star/star24_off");
      }
    }
  }
}
</style>
