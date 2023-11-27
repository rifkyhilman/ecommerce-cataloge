<template>
  <div id="app">
    <div class="container">
      <ProductDisplay />
    </div>
  </div>
</template>

<script>
import ProductDisplay from './components/ProductDisplay.vue';


export default {
  name: 'App',
  components: {
    ProductDisplay
  }
}
</script>

<style>
  
  @import './assets/style/page.css';
  
</style>
