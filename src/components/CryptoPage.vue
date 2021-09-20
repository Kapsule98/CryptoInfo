<template>
  <div class="container">
    <div @click="moreInfo(coin)" class="crypto" v-for="coin,idx in cryptoData" :key=idx>
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
      crypto:[],
      isModalVisible:false,
    }
  },
  created() {
    this.getData()
  },
  // mounted: function () {
  //   window.setInterval(() => {
  //     this.getData()
  //   }, 100000)
  // },
  computed: {
    cryptoData: function () {
      return this.crypto
    }
  },
  methods:{
    getData(){
      const url = BASE_URL + "/cryptolist"
      axios.get(url).then(res => {
        this.crypto = res.data.data
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
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items:center;
}
.crypto{
  height: 14rem;
  width: 21rem;
  border:1px solid peachpuff;
  padding: 3rem;
  background-color: antiquewhite;
}
</style>