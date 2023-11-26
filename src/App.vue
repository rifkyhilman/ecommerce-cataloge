<template>
  <div id="app">
    <div class="container-main">
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

  body{
    margin: 0;
    padding: 0;
  };

  .container-main {
    background-color: var(--purple);
  }
</style>
