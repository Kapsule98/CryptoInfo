<template>
    <div class="container">
        <div class="header">
            <h4>{{coin.name}}</h4>
        </div>
        <div class="body">
            Body
        </div>
        <div class="footer">
            Footer
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { BASE_URL } from '../utils/constants'
export default {
    data(){
        return {
            cryptoinfo:{}
        }
    },
    props:["coin"],
    mounted() {
        this.getCoin()
        // console.log(this.coin)
    },
    methods:{
        getCoin(){
            const url = BASE_URL + `crypto/${this.coin.id}`
            axios.get(url).then(res => {
                if(res.status === 200) {
                    console.log(res.data.status)
                    this.cryptoinfo = res
                } else {
                    alert("Something went wrong")
                }
                
            }).catch(err => {
                console.log(err)
            })
        }
    }
}
</script>
<style scoped>
.container{
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
}
.header{
    background-color: papayawhip;
}
.body{
    background-color: powderblue;
}
.footer{
    background-color: pink;
}
</style>