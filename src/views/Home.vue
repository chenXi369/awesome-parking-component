<template>
  <div class="home flex flex-column">
    <img alt="" src="../assets/bg.jpg" style="width: 100%; height: 200px">
    <plate-number :pageState="pageState" :carnum="carnum" @getCarNum="getCarNum"></plate-number>

    <section class="flex flex-center" v-if="showLoading">
      <loading></loading>
    </section>

    <button @click="checkParkInfo" style="height: 40px">查停车缴费</button>
  </div>
</template>

<script>
// @ is an alias to /src
import PlateNumber from '@/components/PlateNumber.vue'
import Loading from '@/components/Loading.vue'

export default {
  name: 'Home',
  components: {
    Loading,
    PlateNumber
  },
  data() {
    return {
      showLoading: false,
      pageState: 0,            // 适用于部分即兴底部增加高度使用
      carnum: ["粤", "B", "", "", "", "", "", ""]
    }
  },
  methods: {
    // 实际这里一般掉获取停车信息的接口
    async checkParkInfo() {
      this.showLoading = true
      await this.finsihLoading()
    },
    finsihLoading() {
      setTimeout(() => {
        this.showLoading = false
      }, 3000)
    },
    getCarNum(val) {
      console.log(val.join(""))
    }
  }
}
</script>

<style lang="less" scoped>
.home {
  width: 100%;
  height: 100%;
}
</style>
