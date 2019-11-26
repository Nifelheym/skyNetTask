<template>
  <div>
    <div class="second-screen" v-if="showScreen">
      <section style="display:inline-flex;">
        <img id="goArrow" src="../assets/arrow2.svg" alt="arrow2" @click="lastStep()">
        <h4>Тариф "{{tarif.title}}"</h4>
        </section>
      <section v-for="(tar, index) in tarif.tarifs" :key="tar[0]" class="tarif-item">
        <p class="tarif-title">{{tar.pay_period}} {{monthWord(tar.pay_period)}}</p>
        <section class="tarif-description" @click="showThirdPage(index)">
          <div>
            <p>{{tar.price / parseInt(tar.pay_period, 10)}} ₽/мес</p>
            <p class="tarifInfo">разовый платёж - {{tar.price}}</p>
            <p class="tarifInfo" v-if="maxPrice - tar.price / tar.pay_period !== 0">
            скидка - {{(maxPrice - tar.price / tar.pay_period) * tar.pay_period}}₽
            </p>
          </div>
          <img id="goArrow" src="../assets/arrow.svg" alt="arrow">
        </section>
      </section>
    </div>
    <thirdScreen v-if="!showScreen"
    :userSelect="select" @blockTwoComplite="showScreen=true">
    </thirdScreen>
  </div>
</template>

<script>
import thirdScreen from './thirdScreen.vue'

export default {
  props: ['tarif'],
  name: 'SecondScreen',
  components: {
    thirdScreen
  },
  data () {
    return {
      maxPrice: 0,
      showSale: true,
      showScreen: true,
      select: {},
      showThirdScreen: false
    }
  },
  methods: {
    monthWord: function (countOfMonth) {
      switch (countOfMonth) {
        case '1':
          return 'месяц'
        case '3':
          return 'месяцa'
        default:
          return 'месяцев'
      }
    },
    searchMaxValue: function () {
      let arrPrice = []
      for (let index in this.tarif.tarifs) {
        arrPrice.push(this.tarif.tarifs[index].price / parseInt(this.tarif.tarifs[index].pay_period, 10))
      }
      this.maxPrice = Math.max(...arrPrice)
    },
    lastStep: function () {
      this.showScreen = false
      this.$emit('blockComplite', true)
    },
    showThirdPage: function (index) {
      this.select = this.tarif.tarifs[index]
      this.showThirdScreen = !this.showThirdScreen
      this.showScreen = !this.showScreen
      // console.log(this.select)
    }
  },
  mounted: function () {
    console.log(this.tarif)
    this.searchMaxValue()
    console.log(this.maxPrice)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  div.second-screen {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  p.tarifInfo{
    font-weight: normal;
  }
  h4 {
    margin: 1em 7em;
  }
</style>
