<template>
  <div id="app">
    <div id="main" v-if="showFirstPage">
      <section v-for="(tarif, index) in tarifs" :key="tarif[0]" class="tarif-item">
        <h4 class="tarif-title" :id ="index">Тариф "{{tarif.title}}"</h4>
        <section class="tarif-description" @click="showSecondPage(index)">
          <div>
            <p :style="{'background-color':colors[index]}" class="tarif-speed">{{tarif.speed}} Мбит/c</p>
            <p class="tarif-price">{{searchMinAndMaxPrice(index)}}</p>
            <p class="options-item" v-for="option in tarif.free_options" :key="option[0]">{{option}}</p>
          </div>
          <img id="goArrow" src="./assets/arrow.svg" alt="arrow">
        </section>
        <a href="https://www.sknt.ru/" class="link-item">узнать подробнее на сайте www.sknt.ru</a>
      </section>
    </div>
    <SecondScr :tarif="tarif" v-if="!showFirstPage" @blockComplite="showFirstPage = true"></SecondScr>
  </div>
</template>

<script>
import axios from 'axios'
import SecondScr from './components/secondScreen.vue'

const urlApi = 'https://www.sknt.ru/job/frontend/data.json'

export default {
  name: 'App',
  components: {
    SecondScr
  },
  data () {
    return {
      tarifs: [],
      tarif: {},
      showFirstPage: true,
      colors: [
        '#70603e', '#19a5e8', '#ee4e31', '#006400', '#c63131', 'orange', 'black', 'red', 'green', 'blue', 'purple', 'yellow'
      ]
    }
  },
  methods: {
    searchMinAndMaxPrice: function (index) {
      let price = []
      for (let tarifIndex in this.tarifs[index].tarifs) {
        // console.log(this.tarifs[index].tarifs[tarifIndex])
        price.push(this.tarifs[index].tarifs[tarifIndex].price / parseInt(this.tarifs[index].tarifs[tarifIndex].pay_period, 10))
      }
      return `${Math.min(...price)} - ${Math.max(...price)} ₽/мес`
    },
    showSecondPage: function (index) {
      this.showFirstPage = !this.showFirstPage
      this.tarif = this.tarifs[index]
      // console.log(this.tarif)
    }
  },
  mounted: function () {
    axios.get(urlApi)
      .then((response) => {
      // console.log(response.data.tarifs)
        this.tarifs = response.data.tarifs
        console.log(this.tarifs)
      })
  }
}
</script>

<style>
body {
  background-color: #efeff4;
  margin: unset;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
section.tarif-item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  background-color: #ffffff;
  margin: 0 0 1em 0;
  width:  100%;
  width:  -moz-available;          /* WebKit-based browsers will ignore this. */
  width:  -webkit-fill-available;  /* Mozilla-based browsers will ignore this. */
  width:  fill-available;
}
.tarif-title {
  margin: 1em 1em;
  width:  100%;
  width:  -moz-available;          /* WebKit-based browsers will ignore this. */
  width:  -webkit-fill-available;  /* Mozilla-based browsers will ignore this. */
  width:  fill-available;
  text-align: initial;
  color: #94c94a;
  border-bottom: 1px solid #cbcacf;
  font-weight: bold;
}
section.tarif-description {
  border-bottom: 1px solid #cbcacf;
  width:  100%;
  width:  -moz-available;          /* WebKit-based browsers will ignore this. */
  width:  -webkit-fill-available;  /* Mozilla-based browsers will ignore this. */
  width:  fill-available;
  margin: 0.1em 1em;;
  font-weight: bold;
  display: flex;
  justify-content: space-between;
}
p.tarif-speed {
  color: #ffffff;
  font-weight: bold;
  width: max-content;
  margin: unset;
  padding: 0.5em 0.5em 0.5em 1.5em;
}
a.link-item {
  text-decoration: unset;
  margin: 1em 1em;
}
img#goArrow {
  width: 1em;
}
</style>
