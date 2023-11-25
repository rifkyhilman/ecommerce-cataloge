<template>
    <div class="container">
        <h1>
            Hello World !
        </h1>  
        <button @click="Tambah">Chekc function</button>
    </div>
</template>

<script>
export default {
    name: "ProductDisplay",
    data () {
        return {
            title: null,
            price: null,
            index: 0,
            description: null
        }
    },
    methods: {
        async fetchFromApi() {
            const apiUrl = process.env.VUE_APP_API_URL
            const api  = await fetch(apiUrl);
            const productData = await api.json();

            console.log(productData);
        },
        Tambah() {
            this.index == 20 ? this.index = 1 : this.index ++

            console.log(this.index);
        }
    }
}
</script>

<style>

</style>