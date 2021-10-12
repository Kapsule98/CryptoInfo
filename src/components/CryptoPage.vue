<template>
  <div class="container">
    <div @click="moreInfo(coin)" class="crypto" v-for="coin,idx in cryptoData" :key=idx>
      <div>{{coin.name}} ({{coin.symbol}}) </div> 
      <div style="margin:1rem;">$ {{ coin.quote.USD.price.toFixed(4)}} </div>
    </div>
  </div>  
</template>

<script>
import axios from 'axios'
import {BASE_URL} from '../utils/constants'
export default {
  data(){
    return {
      crypto:[],
      isModalVisible:false,
    }
  },
  created() {
    this.getData()
  },
  // refresh data every x (1000)interval
  mounted: function () {
    window.setInterval(() => {
      this.getData()
    }, 1000000)
  },
  computed: {
    cryptoData: function () {
      return this.crypto
    }
  },
  methods:{
    getData(){
      console.log(BASE_URL)
      const url = BASE_URL + "/cryptolist"
      console.log(url)
      axios.get(url).then(res => {
        this.crypto = res.data.data
        console.log(this.crypto)
      }).catch(err => {
        console.log(err)
      })
    },
    moreInfo(coin){
      this.$router.push({name:'coin', params:{coin:coin}})

    }
  }
}
</script>

<style scoped>
.container{
    max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
.crypto{
 background-color: dodgerblue;
  color: white;
  padding: 1rem;
  height: 4rem;
}
</style>