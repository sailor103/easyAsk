<template>
  <div id="app">
    <div class="intro" v-if="sta == 0">
      <p>亲爱的同学：</p>
      <p>您好！本人系武汉大学马克思主义学院15级研究生，<!--
        -->为完成大学生执政党认同现状的相关理论分析而进行此次问卷调查。<!--
        -->执政党认同是指对执政党的肯定性心理和建设性的行为支持。<!--
        -->本次调查不计名，答案没有对错，纯属学术研究，<!--
        -->绝不会透露您的个人信息，您可以放心表达您的真实想法。</p>
      <p>为确保研究结果真实可信，希望您能认真对待。您只需约5分钟即可完成问卷，谢谢合作。</p>
    </div>
    <div class="thanks" v-if="sta == 2">
      非常感谢您的配合，您的态度对我的研究非常有用。
    </div>

    <!--进度条-->
    <div class="prcbar" v-if="sta == 1">
      <div
        class="prc"
        :style="{width: current/(qa.length-1.0) * 100.0 +'%'}"></div>
    </div>

    <!--问题组件-->
    <div class="qaBox" v-if="sta == 1">
      <item
        v-for="(q, i) in qas"
        v-if="q.show"
        :key="i"
        @setAns="onSetAns"
        :qaIndex="i"
        :qaDet="q">
      </item>
    </div>

    <div
      class="start btn"
      @click="startQs"
      v-if="sta == 0">开始答题</div>

    <div class="nav" v-if="sta == 1">
      <div
        class="prev btn"
        @click="onPrev"
        :class="{disable: current == 0}">上一题</div>
      <div
        class="next btn"
        @click="onNext"
        :class="{disable: hideNext}">
        下一题</div>
    </div>

    <div class="submit btn" v-if="sta == 1 && ans.length == qa.length">提交问卷</div>

  </div>
</template>

<script>
/* eslint eqeqeq:0 */
/* eslint no-param-reassign: 0 */
import item from './components/item';
import qa from './storage/qs';

export default {
  name: 'App',
  components: {
    item,
  },
  data() {
    return {
      sta: 1, // 0 未开始 1 正在进行 2 结束
      qa,
      qas: [],
      ans: [], // 用户答案
      current: 0,
    };
  },
  created() {
    this.qas = this.qa.map((i, index) => {
      if (index == 0) {
        i.show = true;
      } else {
        i.show = false;
      }
      return i;
    });
  },
  computed: {
    hideNext() {
      if (this.current == this.qas.length - 1) {
        return true;
      }
      if (this.ans[this.current] && this.ans[this.current].qs) {
        return false;
      }
      return true;
    }
  },
  methods: {
    startQs() {
      this.sta = 1;
    },
    onSetAns(a) {
      if (this.qas[a.qaIndex].s) { // 单选
        this.ans.splice(a.qaIndex, 1, {
          qx: a.qaIndex,
          qs: `sel_${a.selIndex}`,
        });
      } else { // 多选
      }
    },
    onPrev() {
      if (this.current > 0) {
        const old = this.qas[this.current];
        old.show = false;

        const n = this.qas[this.current - 1];
        n.show = true;

        this.qas.splice(this.current, 1, old);
        this.qas.splice(this.current - 1, 1, n);

        this.current = this.current - 1;
      }
    },
    onNext() {
      if (!this.hideNext) { // 还缺判断
        const old = this.qas[this.current];
        old.show = false;

        const n = this.qas[this.current + 1];
        n.show = true;

        this.qas.splice(this.current, 1, old);
        this.qas.splice(this.current + 1, 1, n);

        this.current = this.current + 1;
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
html,body {
  -webkit-user-select: none;
  font-family: 'PingFang SC', 'Comic Sans MS', "幼圆", "黑体", sans-serif;
  background: #eee;
}
#app {
  width: 100%;
  box-sizing: border-box;
  padding: 20px;
  max-width: 640px;
  margin: 0 auto;
}

.btn {
  height: 45px;
  line-height: 45px;
  background: #44b549;
  border-radius: 5px;
  text-align: center;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.btn.disable {
  background: grey;
  color: #aaa;
}

.intro {
  width: 100%;
  margin: 0 auto;
  background: #fff;
  box-sizing: border-box;
  padding: 20px;
  font-size: 15px;
}
.intro p {
  margin-bottom: 10px;
}
.start {
  margin-top: 30px;
}
.prcbar {
  margin-bottom: 20px;
  width: 100%;
  height: 10px;
  background: #fff;
  border-radius: 5px;
}
.prc {
  width: 50%;
  height: 10px;
  background: #44b549;
  border-radius: 5px;
}
.nav {
  width: 100%;
  position: relative;
  height: 45px;
  margin: 20px 0;
}
.nav .prev {
  width: 45%;
  position: absolute;
  top: 0;
  left: 0;
}
.nav .next {
  width: 45%;
  position: absolute;
  top: 0;
  right: 0;
}
</style>
