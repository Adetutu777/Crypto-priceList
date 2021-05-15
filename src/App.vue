<template>
 <h1 class="text-center pt-5"> <b> Trending Crypto Coin </b> </h1>

 
<div class="container table-responsive py-5" > 
<div class="row">

  <div class="form-group has-search pb-5" style="margin: 0 auto">
    <input type="text" class="form-control cont-from"  placeholder="Search" v-model="search" />
  </div>

        <div class="container">
      <div class="row pb-2">
      <div class="col-md-12">
    <p class="text-center" v-if = myCrypto.loading> Loading...</p>

        <p v-if=myCrypto.error> Something went wrong
        <button @click= "myCoin()" type="button"> try again </button>
        </p>
        </div>

       
        </div>
        </div>

          <!-- <div v-if= "myCryto.loaded && myCrypto.data.length ===0"> There are no result for your search
       </div> -->

<table class="table table-bordered table-hover"  >
  <thead class="thead-dark">
    <tr>
         
         <th scope="col">Logo</th>
        <th scope="col">Name</th>
      <th scope="col">Symbol</th>
      <th scope="col">Current Price</th>
      <th scope="col">% movement</th>
      <th scope="col">Mkt. Volume</th>
    
    </tr>
  </thead>
  <tbody>
    <tr class="text-white" v-for= "data in matchNames" :key="data.id">
    
 
     <td><img class="img-img" :src="data.image" alt="cryptoImage"></td>
      <td>{{data.name}}</td>
      <td>{{data.symbol}}</td>
      <td>{{data.current_price}}</td>
      <td>{{data.ath_change_percentage}}</td>
      <td>{{data.total_volume}}</td>
    </tr>
    
  </tbody>
</table>
</div>



</div>

  


</template>

<script>

import {computed, onMounted, ref, reactive } from 'vue';
import axios from 'axios'

export default {
  name: "App",
  
  setup() {
              const myCrypto = reactive({
            data: [],
            loading: null,
            error: false,
          loaded:null,
          firstTimeLoaded:false,
          });

    const search =ref('')

    
      let myCoin = async ()=>{
       myCrypto.loading =  true
        myCrypto.error = false
        myCrypto.loaded= false
    
     try{
          let result = await axios.get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')

         myCrypto.data = result?.data  
             myCrypto.loaded=true         
         }

         catch{
            myCrypto.error = true
            }

             finally {
                  myCrypto.loading = false
            }
      }

          onMounted(myCoin())

      const matchNames = computed(() => {
      
        return myCrypto.data.filter((token) => {
          return (
            token?.name
              .toLowerCase()
              .indexOf(search.value.toLowerCase()) != -1
            
          );
        });
  });
    
   return { matchNames, search, myCrypto, myCoin}
  }
  

  
}
</script>

<style>

#app {
    color: white;
}

.img-img{
width:30px;
}

.cont-from{
background: rgb(245, 230, 230) ;
}

</style>
