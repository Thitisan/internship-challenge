
<template>
  <div>
    <Header/>
    <p></p>
    <vendingMachine/>
  </div>
</template>

<script>
/* eslint-disable */
import vendingMachine from './components/vendingMachine.vue'
import Header from './components/header.vue'
export default {
  name: 'App',
  components: {
    vendingMachine,Header:Header
  },
 
}
</script>

<style>
</style>
