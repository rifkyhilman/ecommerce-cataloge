<template>
    <div class="container-card">
       <div class="image-card">
            <img src="https://i.pinimg.com/originals/62/98/b0/6298b026a65cf80bcf9dce061e9b79c9.png" />
       </div>
       <div class="content-card">
            <div class="content-card-title">
                Baju Bola 
            </div>
           <button @click="Tambah">Chekc function</button>
       </div> 
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

<style scoped>
@import '../assets/style/page.css';

.container-card {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--white);
    /* display: flex;
    position: relative;
    z-index: 2; */
    width: 80%;
    height: 70vh;
    border-radius: 10px;
    padding: 48px 32px;box-shadow: 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -2px rgba(0,0,0,.1);
}

.image-card img {
    width: 200px;
    height: 200px;
}

.content-card {
    width: 100%;
}

</style>