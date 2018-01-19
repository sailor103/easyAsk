<template>
  <div class="qawraper">
    <div class="big">{{qaDet.b}}</div>
    <div class="title">{{qaDet.m}}</div>
    <div class="sels">
      <div
        class="btn"
        v-for="(item, index) in qaDet.sel"
        :class="{sel: item.selected == true}"
        @click="setAns(index)"
        :key="index">
        {{item.text}}
      </div>
    </div>
  </div>
</template>

<script>
/* eslint no-param-reassign: 0 */
export default {
  name: 'item',
  props: {
    qaDet: Object,
    qaIndex: Number,
  },
  computed: {
  },
  created() {
  },
  methods: {
    setAns(sindex) {
      this.$emit('setAns', {
        qaIndex: this.qaIndex,
        selIndex: sindex,
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .btn {
    margin: 15px 0;
  }
  .big {
    font-size: 24px;
  }
  .title {
    font-size: 20px;
    margin-top: 20px;
  }
</style>
