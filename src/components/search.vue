<template>
  

  export const formatMoney =(amt)=> {
return (
new Intl.Number("en-US", {style: 'currency', currency: 'USD'} ).format(amt)
)
}


</template>

<script>
export default {

}
</script>

<style>

</style>