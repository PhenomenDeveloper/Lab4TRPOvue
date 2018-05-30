<template>
  <div class="hello">
    <span class="article"><strong>Криптовалюта</strong></span>
    <span class="rate"><strong>Курс</strong></span>
    <div id="crypto-container" v-for="(value, key, id) in cryptos" :key="id">
      <span class="left">1 {{ key }}</span>
      <span class="right">${{ value.USD }}</span>
    </div>
  </div>
</template>
<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data: () => ({
    cryptos: [],
    errors: [],
    btc: 'BTC',
    eth: 'ETH',
    iot: 'IOT',
    usd: 'USD'
  }),

  created () {
    axios.get('https://min-api.cryptocompare.com/data/pricemulti?fsyms=BTC,ETH,IOT&tsyms=USD')
      .then(response => {
        this.cryptos = response.data
      })
      .catch(e => {
        this.errors.push(e)
      })
    setInterval(this.resp, 10000)
  },
  methods: {
    resp: function () {
      console.log('func')
      axios.get('https://min-api.cryptocompare.com/data/pricemulti?fsyms=BTC,ETH,IOT&tsyms=USD')
        .then(response => {
          if (this.cryptos[this.btc][this.usd] !== response.data[this.btc][this.usd]) {
            this.cryptos[this.btc][this.usd] = response.data[this.btc][this.usd]
          }
          if (this.cryptos[this.eth][this.usd] !== response.data[this.eth][this.usd]) {
            this.cryptos[this.eth][this.usd] = response.data[this.eth][this.usd]
          }
          if (this.cryptos[this.iot][this.usd] !== response.data[this.iot][this.usd]) {
            this.cryptos[this.iot][this.usd] = response.data[this.iot][this.usd]
          }
        })
        .catch(e => {
          this.errors.push(e)
        })
    }
  }

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  body {
    background: #f1f1f1;
  }

  div#crypto-container {
    background:black;
    width: 70%;
    color : white;
    margin: 0 auto 4px auto;
    padding: 1em;
    box-shadow: 1px 1px 0 lightgrey;
  }

  .article {
    margin-left: 183px;
  }

  .rate {
    margin-left: 790px;
  }

  span.left {
    font-weight: bold;
  }

  span.right {
    float:right;
  }
</style>
