<template>
  <div>
      <b-container class="bv-example-row">
      <b-row>
        <!-- mechine and products -->
        <b-col cols="12" md="8">
          <b-row cols="3" md="2">
            <b-col v-for="product in products" :key="product.id">
              <b-col @click="pay(product.name,product.price,product.in_stock)">
                <b-img fluid v-bind:src="product.image"/>
                <p class="text-center">{{product.name}} {{product.price}} Bath</p>
              </b-col>
            </b-col>
          </b-row>
        </b-col>
        <!-- payment area -->
        <b-col cols="6" md="4">
          <b-row>
            <b-button pill variant="primary" class="m-1" @click="insertCoin(1)">1 Bath</b-button>
            <b-button pill variant="primary" class="m-1" @click="insertCoin(2)">2 Bath</b-button>
            <b-button pill variant="primary" class="m-1" @click="insertCoin(5)">5 Bath</b-button>
            <b-button pill variant="primary" class="m-1" @click="insertCoin(10)">10 Bath</b-button>
          </b-row>
          <b-form-text>Insert :{{coin}}</b-form-text>
          <b-form-text>Total :{{totalCoin}}</b-form-text>
          <b-form-text>Change:{{changeCoin}}</b-form-text>
          <b-form-text>you get :{{productName}}</b-form-text>
          <b-button pill variant="outline-primary" @click="refund()">refund</b-button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios'
export default {
    name:'VendingMachine',

    mounted(){
        this.getproductdata();
    },
    methods:{
    //get api
        getproductdata(){
            axios.get('https://www.mocky.io/v2/5c77c5b330000051009d64c9').then(response =>{
                this.products=response.data.data
            })
        },
        insertCoin(res){
            this.coin.push(res)
            this.totalCoin = res+this.totalCoin
        },
        pay(name,price,in_stock){
        // check stock
        //set changeCoin=0
            this.changeCoin = []     
            if(in_stock == true){
                // check totalCoin > price
                if(this.totalCoin >= price){
                    this.totalCoin = this.totalCoin-price
                    this.remain = this.totalCoin
                    this.productName = name
                //ทอนเงิน
                while(this.remain >0){
                    if(this.remain >=10){
                        this.changeCoin.push(10)        
                        this.remain = this.remain-10    
                        this.totalCoin = 0
                        this.coin=[]
                    }else if(this.remain >=5){
                        this.changeCoin.push(5)        
                        this.remain = this.remain-5
                        this.totalCoin = 0
                        this.coin=[]
                    }else if(this.remain >=2){
                        this.changeCoin.push(2)        
                        this.remain = this.remain-2
                        this.totalCoin = 0
                        this.coin=[]
                    }else if(this.remain >=1){
                        this.changeCoin.push(1)        
                        this.remain = this.remain-1
                        this.totalCoin = 0
                        this.coin=[]
                    }else{
                        this.remain = 0
                    }
                }        
                }else{
                    this.productName = "not enough money"
                }
            }else{
                this.productName = "out of stock"
            }
        },
        refund(){
            this.changeCoin = this.coin
            this.totalCoin = 0
            this.coin=[]
        }
    },
    data(){
        return{
            coin:[],
            changeCoin:[],
            productName:'',
            remain:0,
            totalCoin:0,
            products:[],
            product:{
                id:0,
                name:'',
                price:0,
                image:'',
                in_stock:Boolean
            }
        }
    }
}
</script>

<style>
#VendingMachine {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>