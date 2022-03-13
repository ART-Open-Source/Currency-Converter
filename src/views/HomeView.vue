<template>
  <div class="home">
    <img alt="logo" src="../assets/logo.jpg" />
    <p>Currency Converter</p>
    <div class="container1">
      <select name="first" id="first" v-model="currency_one">
        <option value="AED">AED</option>
        <option value="ARS">ARS</option>
        <option value="AUD">AUD</option>
        <option value="BGN">BGN</option>
        <option value="BRL">BRL</option>
        <option value="BSD">BSD</option>
        <option value="CAD">CAD</option>
        <option value="CHF">CHF</option>
        <option value="CLP">CLP</option>
        <option value="CNY">CNY</option>
        <option value="COP">COP</option>
        <option value="CZK">CZK</option>
        <option value="DKK">DKK</option>
        <option value="DOP">DOP</option>
        <option value="EGP">EGP</option>
        <option value="EUR">EUR</option>
        <option value="FJD">FJD</option>
        <option value="GBP">GBP</option>
        <option value="GTQ">GTQ</option>
        <option value="HKD">HKD</option>
        <option value="HRK">HRK</option>
        <option value="HUF">HUF</option>
        <option value="IDR">IDR</option>
        <option value="ILS">ILS</option>
        <option value="INR">INR</option>
        <option value="ISK">ISK</option>
        <option value="JPY">JPY</option>
        <option value="KRW">KRW</option>
        <option value="KZT">KZT</option>
        <option value="MXN">MXN</option>
        <option value="MYR">MYR</option>
        <option value="NOK">NOK</option>
        <option value="NZD">NZD</option>
        <option value="PAB">PAB</option>
        <option value="PEN">PEN</option>
        <option value="PHP">PHP</option>
        <option value="PKR">PKR</option>
        <option value="PLN">PLN</option>
        <option value="PYG">PYG</option>
        <option value="RON">RON</option>
        <option value="RUB">RUB</option>
        <option value="SAR">SAR</option>
        <option value="SEK">SEK</option>
        <option value="SGD">SGD</option>
        <option value="THB">THB</option>
        <option value="TRY">TRY</option>
        <option value="TWD">TWD</option>
        <option value="UAH">UAH</option>
        <option value="USD">USD</option>
        <option value="UYU">UYU</option>
        <option value="VND">VND</option>
        <option value="ZAR">ZAR</option>
      </select>
      <input type="number" name="input-one" id="input-one" v-model="amountOne" @change="fetchData()"/>
    </div>
    <div class="container2">
      <button @click="switchValues()">Switch</button>
      <p id="BaseValue">1 {{ currency_one }} = {{ rate }} {{ currency_two }}</p>
    </div>
    <div class="container3">
      <select name="second" id="second" v-model="currency_two">
          <option value="AED">AED</option>
          <option value="ARS">ARS</option>
          <option value="AUD">AUD</option>
          <option value="BGN">BGN</option>
          <option value="BRL">BRL</option>
          <option value="BSD">BSD</option>
          <option value="CAD">CAD</option>
          <option value="CHF">CHF</option>
          <option value="CLP">CLP</option>
          <option value="CNY">CNY</option>
          <option value="COP">COP</option>
          <option value="CZK">CZK</option>
          <option value="DKK">DKK</option>
          <option value="DOP">DOP</option>
          <option value="EGP">EGP</option>
          <option value="EUR">EUR</option>
          <option value="FJD">FJD</option>
          <option value="GBP">GBP</option>
          <option value="GTQ">GTQ</option>
          <option value="HKD">HKD</option>
          <option value="HRK">HRK</option>
          <option value="HUF">HUF</option>
          <option value="IDR">IDR</option>
          <option value="ILS">ILS</option>
          <option value="INR">INR</option>
          <option value="ISK">ISK</option>
          <option value="JPY">JPY</option>
          <option value="KRW">KRW</option>
          <option value="KZT">KZT</option>
          <option value="MXN">MXN</option>
          <option value="MYR">MYR</option>
          <option value="NOK">NOK</option>
          <option value="NZD">NZD</option>
          <option value="PAB">PAB</option>
          <option value="PEN">PEN</option>
          <option value="PHP">PHP</option>
          <option value="PKR">PKR</option>
          <option value="PLN">PLN</option>
          <option value="PYG">PYG</option>
          <option value="RON">RON</option>
          <option value="RUB">RUB</option>
          <option value="SAR">SAR</option>
          <option value="SEK">SEK</option>
          <option value="SGD">SGD</option>
          <option value="THB">THB</option>
          <option value="TRY">TRY</option>
          <option value="TWD">TWD</option>
          <option value="UAH">UAH</option>
          <option value="USD">USD</option>
          <option value="UYU">UYU</option>
          <option value="VND">VND</option>
          <option value="ZAR">ZAR</option>
      </select>
      <input type="number" id="amount-two" placeholder="0" disabled v-model="amountTwo">
    </div>
    <div class="container">
      <p id="LastlyUpdate">Last Update : {{data.time_last_update_utc}}</p>
    </div>
  </div>
</template>

<script>

export default {
  name: "HomeView",
  data(){
    return {
      data:[],
      currency_one: "USD",
      currency_two: "EUR",
      rate: "",
      amountOne: 1,
      amountTwo: 0,
    }
  },
  methods:{
    fetchData(){
      const KEY = "Your Api Key"
      fetch(`https://v6.exchangerate-api.com/v6/${KEY}/latest/${this.currency_one}`)
      .then(response => response.json())
      .then(data => {
        console.log(data);
        this.data = data
        this.rate = data.conversion_rates[this.currency_two];
        this.amountTwo = this.amountOne * this.rate.toFixed(2);
      });
    },

      switchValues() {
      const temp = this.currency_one;
      this.currency_one = this.currency_two;
      this.currency_two = temp;
      this.calculateResults();
    },
    
  },
  mounted(){
    this.fetchData();
  }
  
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Fruktur&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Ubuntu&display=swap');
*{
    font-family: 'Poppins' ,'sans-serif' ;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    scroll-behavior: smooth;

}
img {
  width:10em;
}
.home {
  display:flex;
  flex-direction:column;
  align-items:center;
  align-content:center
}
button {
  margin-left: 3em;
  padding-right: 1em;
  padding-left: 1em;
}
input {
  text-align:center
}
</style>
