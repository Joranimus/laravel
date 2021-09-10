<template>
  <div class="stocks">
    <div class="button" @click="getData">get data</div>
    <div v-if="stock">
      <stockCard v-for="item in stock" :key="item.meta.symbol"
        v-bind:symbol="item.meta.symbol"
        v-bind:currency="item.meta.currency"
        v-bind:open="item.values[0].open"
        v-bind:close="item.values[0].close"
        v-bind:volume="item.values[0].volume"
      />
    </div>
    

    
  </div>
</template>

<script>
// @ is an alias to /src
import stockCard from './stockCard.vue'
const axios = require('axios');

export default {
  name: 'Stocks',
  components: {
    stockCard
  },
  data(){
    return{
      stock:{},
      time:""
    }
  },
  mounted() {
    if (localStorage.getItem('stock')) {
      this.stock = JSON.parse(localStorage.getItem('stock'));
    };
    if(localStorage.getItem('stocktime')){this.time = localStorage.getItem('stocktime')};
    if((Date.now() - this.time) >= (1000*60*60)){this.getData};
  },
  watch: {
    stock(data) {
      const parsed = JSON.stringify(data);
      localStorage.setItem('stock', parsed);
    },
    time(data){
      localStorage.setItem('stocktime', data);
    }
  },
  methods:{
    getData(){
      axios.get('https://api.twelvedata.com/time_series?symbol=AAPL,MSFT,SBUX&interval=1min&apikey=94319ea3f0d54c3ea662fb37be4aaaa4')
        .then(response => {
          this.stock = response.data
          this.time = Date.now();
          console.log(this.stock);
        }) 
        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .then(function () {
          // always executed
        });
    }
  }
}
</script>

<style lang="scss">
.stocks{
  padding: 20px;
}

</style>
