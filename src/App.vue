<template>
<h2 class="text-center mt-5 pb-4"> <b> Top 100 Crypto Coin </b> </h2>
  <div class="container">
  <div class="row">
  <div class="col-md-4"></div>
  <div class="col-md-4">
  <form>
    <!-- search input -->
    <div class="form-outline mb-4">
      <input type="text" class="form-control cont-from" style="margin: 0 auto; width:70%" placeholder="search name" v-model="search" required/>
    </div>
  </form></div>
  <div class="col-md-4"></div>
  </div>
</div>




<div class="container table-responsive py-5" >
<div class="row">

  <!-- <div class="form-group has-search pb-5" style="margin: 0 auto">
    <input type="text" class="form-control cont-from"  placeholder="Search" v-model="search" />
  </div> -->

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
      <th clas="text-center" scope="col">Symbol</th>
      <th scope="col">Current Price</th>
      <th scope="col">% in 24hrs</th>
      <th scope="col">Mkt. cap</th>
    
    </tr>
  </thead>
  <tbody>
    <tr class="text-white datanames" v-for= "data in matchNames" :key="data.id">
    
 
     <td class="text-center"><img class="img-img" :src="data.image" alt="cryptoImage"></td>
      <td class="">{{data.name}}</td>
      <td clas="text-center">{{data.symbol}}</td>
      <td> ${{data.current_price.toLocaleString()}}</td>
      <td>
      <span :class = "`${data.price_change_percentage_24h> 0 ? 'green' : 'red'}`">
       {{data.price_change_percentage_24h.toFixed(2)}}%
      </span>

      </td>

      <td>${{data.market_cap.toLocaleString()}}</td>
    </tr>
    
  </tbody>
</table>
</div>



</div>

  <footer>
<!-- Copyright -->
  <div class="footer-copyright text-center py-3 my-info">© 2023 Copyright:
    <a class=" info-text" style="text-decoration:none" href="https://adetutu.netlify.app/"> <i>Adetutu</i></a>
  </div>
  <!-- Copyright -->
</footer>
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
    
     
    const formatMoney =(amt)=> {
      return (
      new Intl.Number("en-US", {style: 'currency', currency: 'USD'} ).format(amt)
      )
}
   return { matchNames, search, myCrypto, myCoin, formatMoney}
  }
  

  
}
</script>

<style>

#app {
    color: white;
}

.datanames:hover{

border: 1.5px solid rgb(235, 11, 78);
}

.img-img{
max-width:30px;
}

.cont-from{
color: black;
font-weight: bold;
}

.green{
color:rgb(19, 236, 19);
}

.red {
color:rgb(255, 6, 6);
}

.info-text{
font-size: 1.2rem;
font-weight: bolder;
color: rgb(141, 133, 253);

}



</style>
