<template>
  <div id="thirdScreen" v-if="showthird">
    <section style="display:inline-flex;">
      <img id="goArrow" src="../assets/arrow2.svg" alt="arrow2" @click="lastStep()">
      <h4>Выбор тарифа</h4>
    </section>
    <section class="tarif-description">
      <div>
        <p class="tarif-title" style="margin:unset;">Тариф "{{userSelect.title}}"</p>
        <p>Период оплаты - {{userSelect.pay_period}} {{monthWord(userSelect.pay_period)}}</p>
        <p>{{userSelect.price / userSelect.pay_period}} ₽/мес</p>
        <p class="payCount">разовый платёж - {{userSelect.price}} ₽</p>
        <p class="payCount">со счёта спишется - {{userSelect.price}} ₽</p>
        <p>вступит в силу - сегодня</p>
        <p>активно до - {{getMonth(userSelect.pay_period)}}</p>
      </div>
      <!-- <img id="goArrow" src="../assets/arrow.svg" alt="arrow2"> -->
    </section>
    <button>Выбрать</button>
  </div>
</template>
<script>
export default {
  name: 'thirdScreen',
  props: ['userSelect'],
  data () {
    return {
      maxPrice: 0,
      showthird: false
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
    lastStep: function () {
      this.showthird = false
      this.$emit('blockTwoComplite', true)
    },
    getMonth: function (month) {
      let newMonth = parseInt(month, 10)
      let date = new Date()
      date.setMonth(date.getMonth() + newMonth)
      let formatDate = ('0' + date.getDate()).slice(-2) + '.' + ('0' + (date.getMonth() + 1)).slice(-2) + '.' + date.getFullYear()
      return formatDate
    }
  },
  mounted: function () {
    console.log(this.userSelect)
    this.showthird = true
  }
}
</script>
<style scoped>
  h4 {
    text-align: center;
  }
  button {
    width: 70%;
    min-height: 3em;
    background-color: #82c12b;
    color: #ffffff;
    margin-top: 1em;
    border: none;
  }
  h4{
    margin: 1em 7em;
  }
  p.payCount{
    font-weight: normal;
  }
  div#thirdScreen{
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  </style>
