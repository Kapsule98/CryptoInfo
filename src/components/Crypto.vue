<template>
    <div class="body">
        <div class="card">
            
            <div > 
                <button class="backbutton" @click="back()">Back</button>
                RANK {{coin.cmc_rank}} 
            </div>
            <div><img v-bind:src="cryptoinfo.logo"/></div>
            <div>{{ cryptoinfo.name}}</div>
            <div>{{cryptoinfo.description}}</div>
            <div class="innercard">
                <div> Date Added </div> 
                <div> {{cryptoinfo.date_added.substring(0,9)}} </div>    
            </div>
            <div class="innercard">
                <div> Total Supply </div> 
                <div> {{coin.total_supply}} </div>    
            </div>
            <div class="innercard">
                <div> Circulating Supply </div> 
                <div> {{coin.circulating_supply}} </div>    
            </div>
        </div>
        <div class="card">
            <div class="cardhead"> Market Info </div>
            <div class="innercard">
                <div> Market Cap </div> 
                <div> {{cap.market_cap.toFixed(2)}} USD</div>
            </div>
            <div class="innercard">
                <div> Price </div> 
                <div> {{cap.price.toFixed(5)}} USD </div>
            </div>
            <div class="innercard">
                <div> Market cap dominance </div> 
                <div> {{cap.market_cap_dominance.toFixed(2)}} %</div>
            </div>
            <div class="innercard">
                <div> Fully diluted market cap </div> 
                <div> {{cap.fully_diluted_market_cap.toFixed(2)}} USD</div>
            </div>
            <div class="cardhead"> Percentage change </div>
            <div class="innercard">
                <div> 1 Hour </div> 
                <div> {{cap.percent_change_1h.toFixed(2)}} % </div>
            </div>
            <div class="innercard">
                <div> 7 Days </div> 
                <div> {{cap.percent_change_7d.toFixed(2)}} %</div>
            </div>
            <div class="innercard">
                <div> 30 Days </div> 
                <div> {{cap.percent_change_30d.toFixed(2)}} %</div>
            </div>
            <div class="innercard">
                <div> 60 Days </div> 
                <div> {{cap.percent_change_60d.toFixed(2)}} %</div>
            </div>
            <div class="innercard">
                <div> 90 Days </div> 
                <div> {{cap.percent_change_90d.toFixed(2)}} %</div>
            </div>
            <!-- 1h {{cap.percent_change_1h}}
            7d {{cap.percent_change_7d}}
            30d {{cap.percent_change_30d}}
            60d {{cap.percent_change_60d}}
            90d {{cap.percent_change_90d}} -->
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { BASE_URL } from '../utils/constants'
export default {
    data(){
        return {
            id:0,
            cryptoinfo:{},
            /*  cryptoinfo
                category: (...)
                date_added: (...)
                description: (...)
                id: (...)
                is_hidden: (...)
                logo: (...)
                name: (...)
                notice: (...)
                platform: (...)
                slug: (...)
                subreddit: (...)
                symbol: (...)
                tag-groups: (...)
                tag-names: (...)
                tags: (...)
                twitter_username: (...)
                urls: (...)
            */
            cap:{},
            /*
            fully_diluted_market_cap: (...)
            last_updated: (...)
            market_cap: (...)
            market_cap_dominance: (...)
            percent_change_1h: (...)
            percent_change_7d: (...)
            percent_change_24h: (...)
            percent_change_30d: (...)
            percent_change_60d: (...)
            percent_change_90d: (...)
            price: (...)
            volume_24h: (...)
            */
            
        }
    },
    props:["coin"],
    /*
    circulating_supply: (...)
    cmc_rank: (...)
    date_added: (...)
    id: (...)
    last_updated: (...)
    max_supply: (...)
    name: (...)
    num_market_pairs: (...)
    platform: (...)
    quote: (...)
    slug: (...)
    symbol: (...)
    tags: (...)
    total_supply: (...)
    */
    mounted() {
        this.getCoin()
        this.id = this.coin.id
        this.cap = this.coin.quote[Object.keys(this.coin.quote)[0]]
        console.log(this.coin)
        console.log(this.cap)
    },
    methods:{
        getCoin(){
            const url = BASE_URL + `/crypto/${this.coin.id}`
            axios.get(url).then(res => {
                if(res.status === 200 && res.data.status.error_code === 0) {
                    console.log(res.data.data)
                    this.cryptoinfo = res.data.data[this.id]
                } else {
                    alert("Something went wrong")
                }
                
            }).catch(err => {
                console.log(err)
            })
        },
        back() {
            this.$router.push('/')
        }
    }
}
</script>
<style scoped>
.body {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
.header{
    display: grid;
    grid-template-columns: 5rem auto;
}
.card{
 background:wheat;  /* fallback for old browsers */
background: -webkit-linear-gradient(to left,wheat, peachpuff);  /* Chrome 10-25, Safari 5.1-6 */
background: linear-gradient(to left, wheat, peachpuff); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */


  color: black;
  padding: 1rem;
  height: auto;
  display: grid;
  grid-template-columns: auto;
  justify-content: center;
  align-content: space-around;
  row-gap: 0.6rem;
}
.innercard{
    display: grid;
    grid-template-columns: 50% 50%;
    row-gap: 2rem;
    padding: 0.3rem;
}
.backbutton{
    position:absolute;
    top: 1rem;
    left:2rem;
     background-color: #e7e7e7; 
     color: black;
  border: 1px solid black;
  padding: 8px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 10px;
  transition-duration: 0.4s;
  box-shadow: 0 2px #999;
}

.backbutton:hover {
  background-color: black; /* Green */
  color: white;
}
.backbutton:active {
  background-color: black;
  color:white;
  box-shadow: 0 1px #666;
  transform: translateY(2px);
}
</style>