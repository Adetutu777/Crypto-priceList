

<template>
 <div class="container">
        <div class="row">
        <div class="col-md-2"></div>
        <div class="col-md-8">
        
        <div class="aa"  :v-show= "loaded">
         <h2 class="text-center pt-5"> <b> Trending Crypto Coin </b> </h2>

         <div class="form-group has-search pb-5" style="margin: 0 auto">
    <input type="text" class="form-control cont-from"  placeholder="Search" v-model="search" />
  </div>

  <!-- table -->

        <div class="table-sec table-responsive py-5">
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
    <tr class="text-white datanames" v-for= "data in matchNames" :key="data.id">
    
 
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
        
        </div>
        <div class="col-md-2"></div>
        </div>

        <div class="row">
         <div class="col-md-12">
    <p class="text-center" v-if = myCrypto.loading> Loading...</p>

        <p v-if=myCrypto.error> Something went wrong
        <button @click= "myCoin()" type="button"> try again </button>
        </p>
        </div>
        </div>
 </div>


</template>
