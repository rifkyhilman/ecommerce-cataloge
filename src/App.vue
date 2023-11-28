<template>
  <div id="app">
    <LoaderPage v-if="isLoaded" />
    <ProductDisplay v-else @sendDataFromChild="getDataFromChild" />
  </div>
</template>

<script>
import LoaderPage from './components/LoaderPage.vue';
import ProductDisplay from './components/ProductDisplay.vue';


export default {
  name: 'App',
  components: {
    LoaderPage,
    ProductDisplay
},
  data() {
    return {
      isLoaded: true
    }
  },
  methods: {
    getDataFromChild(val) {
      this.isLoaded = val;
    }
  }
}

</script>

<style>
  
  @import './assets/style/page.css';
  
</style>
