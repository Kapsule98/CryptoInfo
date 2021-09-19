<template>
  <div>
    <div v-for="coin,idx in cryptoData" :key=idx>
      {{coin.name}} - {{ coin.quote.USD.price}}
    </div>
  </div>  
</template>

<script>
import axios from 'axios'
import {BASE_URL} from '../utils/constants'
export default {
  data(){
    return {
      crypto:[]
    }
  },
  created() {
    this.getData()
  },
  mounted: function () {
    window.setInterval(() => {
      this.getData()
    }, 10000)
  },
  computed: {
    cryptoData: function () {
      return this.crypto
    }
  },
  methods:{
    getData(){
      const url = BASE_URL + "/crypto"
      axios.get(url).then(res => {
        this.crypto = res.data.data
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>

<style scoped>

</style>