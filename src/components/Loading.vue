<template lang="pug">
#loadingCom(
  v-show="show",
  @touchmove.stop.prevent="")
  .loadingBox
    .loadimg
      <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 32 32" style="enable-background:new 0 0 50 50;" xml:space="preserve">
        <circle cx="16" cy="3" r="0.477101">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(45 16 16)" cx="16" cy="3" r="0">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.125s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(90 16 16)" cx="16" cy="3" r="0">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.25s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(135 16 16)" cx="16" cy="3" r="0.00822592">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.375s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(180 16 16)" cx="16" cy="3" r="0.632891">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.5s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(225 16 16)" cx="16" cy="3" r="1.94799">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.625s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(270 16 16)" cx="16" cy="3" r="2.8489">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.75s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(315 16 16)" cx="16" cy="3" r="2.01891">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.875s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
        <circle transform="rotate(180 16 16)" cx="16" cy="3" r="0.632891">
          <animate attributeName="r" values="0;3;0;0" dur="1s" repeatCount="indefinite" begin="0.5s" keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" calcMode="spline"></animate>
        </circle>
      </svg>
    slot
</template>

<script>
  export default {
    name: 'Loading',
    props: {
      show: Boolean,
    },
    computed: {
    },
    methods: {
    },
  };
</script>

<style lang="stylus" scoped>
#loadingCom
  position fixed
  width 100%
  height 100%
  top 0
  left 0
  .loadingBox
    width 120px
    height 90px
    position absolute
    top 0
    bottom 0
    left 0
    right 0
    margin auto
    border-radius 5px
    background #333
    text-align center
    padding 10px 0
    box-sizing border-box
    -webkit-box-sizing border-box
    font-size 14px
    color #fff
    svg
      width 36%
      fill #fff
      margin-bottom 5px
</style>
