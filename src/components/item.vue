<template>
  <div class="qawraper">
    <div class="big">{{qaDet.b}}</div>
    <div class="title">{{qaDet.m}}</div>
    <div class="sels">
      <div
        class="btn"
        v-for="(item, index) in sels"
        :class="{sel: item.sel == true}"
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
  data() {
    return {
      sels: [],
    };
  },
  computed: {
  },
  created() {
    this.sels = this.qaDet.sel.map((i) => {
      const o = {
        sel: false,
        text: i,
      };
      return o;
    });
  },
  methods: {
    setAns(sindex) {
      if (this.qaDet.s) { // 单选
        this.sels.forEach((i) => {
          i.sel = false;
        });
        this.sels.splice(sindex, 1, {
          sel: true,
          text: this.sels[sindex].text,
        });
        this.$emit('setAns', {
          qaIndex: this.qaIndex,
          selIndex: sindex,
        });
      } else { // 多选
        this.sels.splice(sindex, 1, {
          sel: !this.sels[sindex].sel,
          text: this.sels[sindex].text,
        });
        this.$emit('setAns', {
          qaIndex: this.qaIndex,
          selIndex: sindex,
          isAdd: this.sels[sindex].sel,
        });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .btn {
    margin: 20px 0;
  }
  .btn.sel {
    border: 2px dashed black;
    background: red;
  }
  .big {
    font-size: 18px;
  }
  .title {
    font-size: 16px;
    margin-top: 20px;
  }
</style>
