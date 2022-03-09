<template>
  <div class="random-with-scope">
    <h1>范围内取随机平均数</h1>
    <div>数据总量：<input v-model.number="zong_liang" type="number"/></div>
    <div>数据个数：<input v-model.number="ge_shu" type="number"/></div>
    <div>误差范围：<input v-model.number="wu_cha" type="number"/></div>
    <div>
      <button @click="randomWithScome" id="pai">拍</button>
    </div>
    <div v-if="jie_guo.length > 0">
      <div>结果输出：</div>
      <div>==============================</div>
      <div>
        <div v-for="(item,index) in jie_guo" :key="index">{{ item }}</div>
      </div>
      <div>==============================</div>
      <div>验证总数(请与原始数据对比)：{{ zong_shu_yan_zheng }}</div>
      <div>==============================</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RandomWithScope',
  props: {},
  data() {
    return {
      zong_liang: 0,
      ge_shu: 0,
      wu_cha: 0,
      jie_guo: [],
      zong_shu_yan_zheng: 0
    }
  },
  methods: {
    randomWithScome() {
      this.jie_guo = []
      let ping_jun_shu = Math.floor(this.zong_liang / this.ge_shu)
      for (let i = 0; i < this.ge_shu; i++) {
        let wu_cha_zhi = Math.floor(Math.random() * this.wu_cha);
        let zeng_jia = Math.random() > 0.5;
        let xin_jia_ru_shu_zi = zeng_jia ? ping_jun_shu + wu_cha_zhi : ping_jun_shu - wu_cha_zhi
        let isExist = false
        this.jie_guo.forEach(item => {
          if (xin_jia_ru_shu_zi === item) {
            isExist = true
          }
        })
        if (this.wu_cha <= 0) {
          isExist = false
        }
        if (isExist) {
          i--
        } else {
          this.jie_guo.push(xin_jia_ru_shu_zi)
        }
      }
      let zong_shu = 0
      this.jie_guo.forEach(item => {
        zong_shu = zong_shu + item
      })
      let zong_wu_cha = this.zong_liang - zong_shu
      let ping_jun_fen_wu_cha = Math.floor(zong_wu_cha / this.ge_shu)
      let yu_shu = zong_wu_cha % this.ge_shu
      for (let i = 0; i < this.jie_guo.length; i++) {
        this.jie_guo[i] = this.jie_guo[i] + ping_jun_fen_wu_cha
        if (i === this.jie_guo.length - 1) {
          this.jie_guo[i] = yu_shu < 0 ? this.jie_guo[i] + yu_shu + this.ge_shu : this.jie_guo[i] + yu_shu
        }
      }
      this.zong_shu_yan_zheng = 0
      this.jie_guo.forEach(item => {
        this.zong_shu_yan_zheng = this.zong_shu_yan_zheng + item
      })

    }
  }
}
</script>

<style scoped>
#pai {
  width: 64px;
  height: 64px;
  font-size: 32px;
  margin: 16px;
}
</style>
