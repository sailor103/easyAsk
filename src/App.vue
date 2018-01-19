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
      <p>非常感谢您的配合，您的态度对我的研究非常有用。</p>
    </div>

    <!--进度条-->
    <div class="prcbar" v-if="sta == 1">
      <div
        class="prc"
        :style="{width: current/(qas.length-1.0) * 100.0 +'%'}"></div>
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

    <div
      class="submit btn"
      @click="onSubmitAns"
      v-if="sta == 1 && showSubmit">提交问卷</div>

    <Loading :show="loadObj.show"> {{loadObj.text}}</Loading>

  </div>
</template>

<script>
/* eslint eqeqeq:0 */
/* eslint no-param-reassign: 0 */
import item from './components/item';
import Loading from './components/Loading';
import qas from './storage/qs';

export default {
  name: 'App',
  components: {
    item,
    Loading,
  },
  data() {
    return {
      sta: 1, // 0 未开始 1 正在进行 2 结束
      qas,
      current: 0,
      loadObj: {
        show: false,
        text: '加载中...',
      },
    };
  },
  created() {
    const APP_ID = 'w1QY6hFcT63xvmFUSV7uvaJk-gzGzoHsz';
    const APP_KEY = 'nSyKSIKJTjFjHorHbgYrBVtr';

    const AV = window.AV;

    AV.init({
      appId: APP_ID,
      appKey: APP_KEY,
    });
  },
  computed: {
    hideNext() {
      if (this.current == this.qas.length - 1) {
        return true;
      }
      const hasSel = this.qas[this.current].sel.some(i => i.selected);
      if (hasSel) {
        return false;
      }
      return true;
    },
    showSubmit() {
      const selArr = this.qas.map(qi => qi.sel.some(i => i.selected));
      return selArr.every(i => i);
    },
  },
  methods: {
    startQs() {
      this.sta = 1;
    },
    onSetAns(a) {
      if (this.qas[a.qaIndex].s) { // 单选
        this.qas[a.qaIndex].sel.forEach((i) => {
          i.selected = false;
        });
        this.$set(this.qas[a.qaIndex].sel, a.selIndex, {
          selected: true,
          text: this.qas[a.qaIndex].sel[a.selIndex].text,
        });
      } else { // 多选
        this.$set(this.qas[a.qaIndex].sel, a.selIndex, {
          selected: !this.qas[a.qaIndex].sel[a.selIndex].selected,
          text: this.qas[a.qaIndex].sel[a.selIndex].text,
        });
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
    onSubmitAns() {
      this.loadObj.show = true;

      const ES = window.AV.Object.extend('easyAsk');
      const eak = new ES();

      this.qas.forEach((q, qinx) => {
        const mysel = [];
        q.sel.forEach((i, ix) => {
          if (i.selected) {
            mysel.push(`sel_${ix}`);
          }
        });
        eak.set(`qestion_${qinx}`, mysel.join(';'));
      });

      eak.save().then((rel) => {
        // 成功保存之后，执行其他逻辑.
        console.log('save success====>', rel);
        this.loadObj.show = false;
        this.sta = 2;
      }, (error) => {
        // 异常处理
        console.log('svae error=======>', error);
      });
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
  height: 40px;
  line-height: 40px;
  background: #44b549;
  border-radius: 5px;
  text-align: center;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  box-sizing: border-box;
}

.btn.disable {
  background: grey !important;
  color: #aaa;
}

.btn.sel {
  border: 2px dashed black;
  background: red;
}

.intro,.thanks {
  width: 100%;
  margin: 0 auto;
  background: #fff;
  box-sizing: border-box;
  padding: 20px;
  font-size: 15px;
}
.intro p,.thanks p {
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
  margin: 30px 0;
}
.nav .prev {
  width: 45%;
  position: absolute;
  top: 0;
  left: 0;
  border-top-left-radius: 50px;
  border-bottom-left-radius: 50px;
  height: 36px;
  line-height: 36px;
  background: #4889db;
}
.nav .next {
  width: 45%;
  position: absolute;
  top: 0;
  right: 0;
  border-top-right-radius: 50px;
  border-bottom-right-radius: 50px;
  height: 36px;
  line-height: 36px;
  background: #4889db;
}
.submit {
  font-size: 20px;
  background: #ff9000;
}
</style>
