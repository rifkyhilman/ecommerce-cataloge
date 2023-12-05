<template>
    <LoaderPage v-if="isLoaded" />
    <div v-else :class="{'container bg-blue':isMen, 'container bg-pink':isWomen, 'container bg-gray':isOther}">
        <div class="overlay">
            <img src="../assets/images/bg-pattern.svg" />
        </div>
        <div v-if="!isOther" class="container-card">
            <div class="image-card">
                <img :src="product.data.image" />
            </div>
            <div class="content-card">
                <div class="content-card-top">
                    <div class="content-card-title">
                        <h3 :class="{'font-navy':isMen , 'font-magenta':isWomen}">
                            {{ product.data.title }}
                        </h3>
                    </div>
                    <div class="content-card-rating">
                        <div class="categeori">
                            <span> {{ product.data.category }} </span>
                        </div>
                        <div class="content-card-rating-detail">
                            <span> {{ product.data.rating.rate }}/5 </span>
                            <div class="content-card-rating-icon">
                                <span :class="{'bg-navy circle':isMen , 'bg-magenta circle':isWomen }"></span>
                                <span :class="{'bg-navy circle':isMen , 'bg-magenta circle':isWomen }"></span>
                                <span :class="{'bg-navy circle':isMen , 'bg-magenta circle':isWomen }"></span>
                                <span :class="{'border-navy circle':isMen , 'border-magenta circle':isWomen }"></span>
                                <span :class="{'border-navy circle':isMen , 'border-magenta circle':isWomen }"></span>
                            </div>
                        </div>
                    </div>
                    <div class="content-card-description">
                    <p> {{ product.data.description }} </p> 
                    </div>
                </div>
                <div class="content-card-bottom">
                    <div class="content-card-price">
                        <span :class="{'font-navy':isMen, 'font-magenta':isWomen}">${{ product.data.price }}</span>
                    </div>
                    <div class="content-card-btn">
                        <button :class="{'content-card-btn-buy bg-navy':isMen, 'content-card-btn-buy bg-magenta':isWomen}">Buy Now</button>
                        <button :class="{'content-card-btn-next border-navy font-navy':isMen, 'content-card-btn-next border-magenta font-magenta':isWomen}" @click="Tambah">Next Product</button>
                    </div>
                </div>
            </div> 
        </div>
        <div v-else class="container-card-unavailable">
            <div class="container-card-unavailable-content">
                <div class="overlay-unavailable">
                    <img src="../assets/images/sad-face.svg" />
                </div>
                <div class="content-card-unavailable">
                    <p>This product is unavailable to show</p>
                    <div class="content-card-unavailable-btn">
                        <button @click="Tambah" class="content-card-unavailable-btn-next"> Next Product </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import LoaderPage from './LoaderPage.vue';

export default {
    name: "ProductDisplay",
    components: {
    LoaderPage
},
    data() {
        return {
            index: 1,
            product: {},
            isMen: null,
            isWomen: null,
            isOther: false,
            isLoaded: true
        };
    },
    created() {
        this.fetchFromApi(this.index);
    },
    watch: {
        index(val) {
            this.handleChange(val);
        }
    },
    methods: {
        handleChange(val) {
            this.fetchFromApi(val);
        },
        async fetchFromApi(val) {
            try {
                const apiUrl = process.env.VUE_APP_API_URL;
                const api = await fetch(apiUrl + val);
                const data = await api.json();


                if (data.category == "men's clothing" || data.category == "women's clothing"){
                    this.product = { data };
                    this.isOther = false;
                    
                    if (data.category === "men's clothing"){
                        this.isMen = true;
                        this.isWomen = false;
                    } else if (data.category === "women's clothing"){
                        this.isWomen = true;
                        this.isMen = false;
                    } 

                } else {
                    this.isOther = true;
                }

                setTimeout(() => {
                    this.isLoaded = false;
                }, 500);
                    
            } catch (error) {
                console.log(error);
            }
        
        },
        Tambah() {
            this.isLoaded = true;
            this.index == 20 ? this.index = 1 : this.index++;
        }
    }
}

</script>

<style>

    .overlay {
        width: 100%;
        position: absolute;
        z-index: 1;
        top: -16%;
        left: 0;
    }

    .overlay img {
        width: 100%;
    }


    .container-card {
        display: grid;
        grid-template-columns: 30% auto;
        gap: 16px;
        width: 80%;
        z-index: 2;
        background-color: var(--white);
        border-radius: 10px;
        padding: 48px 40px;
        box-shadow: 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -2px rgba(0,0,0,.1);
    }


    .image-card {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .image-card img {
        width: 80%
    }

    .content-card {
        display: flex;
        width: 100%;
        flex-direction: column;
        justify-content: space-between;
    }

    .content-card-title h3 {
        font-size: 32px;
        font-weight: 600;
    }


    .content-card-rating {
        display: flex;
        width: 100%;
        justify-content: space-between;
        margin-top: 17px;
        font-size: 16px;
        color: var(--dark-gray);
        padding-bottom: 14px;
        border-bottom: 1px solid var(--gray);
    }

    .content-card-rating-detail {
        display: grid;
        grid-template-columns: auto auto;
        gap: 5px;
        margin-right: 10px;
    }

    .content-card-rating-icon {
        display: flex;
        gap: 3px;
    }

    .content-card-description {
        width: 100%;
        margin: 24px 0 24px 0;
        font-size: 18px;
        color: var(--black);
    }

    .content-card-price {
        padding-top: 16px;
        border-top: 1px solid var(--gray);
    }

    .content-card-price span {
        font-size: 32px;
        font-weight: 600;
    }

    .content-card-btn {
        margin-top: 16px;
    }

    .content-card-btn-buy {
        color: var(--white);
        border: none;
        width: 35%;
        height: 36px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
    }

    .content-card-btn-buy:hover {
        background-color: var(--black);
        color: var(--white);
    }

    .content-card-btn-next {
        background-color: var(--white);
        border: none;
        width: 35%;
        height: 36px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
        margin-left: 12px;
    }

    .content-card-btn-next:hover {
        background-color: var(--black);
        color: var(--white);
    }

    .content-card-bottom {
        margin-top: 80px;
    }

    /* UNVAILABLE */

    .container-card-unavailable {
        display: flex;
        position: relative;
        z-index: 2;
        width: 80%;
        height: 70vh;
        background-color: var(--white);
        border-radius: 10px;
        padding: 48px 32px;
        box-shadow: 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -2px rgba(0,0,0,.1);
    }

    .container-card-unavailable-content {
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }

    .overlay-unavailable {
        position: absolute;
        z-index: 1;
        top: 15%;
        left: 10%;
    }

    .overlay-unavailable img {
        width: 100%;
    }

    .content-card-unavailable {
        display: flex;
        width: 100%;
        flex-direction: column;
        justify-content: space-between;
        text-align: center;
        z-index: 2;
        font-size: 20px;
        font-weight: 400;
    }
    
    .content-card-unavailable-btn {
        margin-top: 16px;
        width: 100%;
        display: flex;
        gap: 12px;
    }

    .content-card-unavailable-btn-next {
        margin: 0 auto;
        border: 2px solid var(--black);
        color: var(--black);
        background-color: var(--white);
        width: 45%;
        height: 36px;
        font-size: 16px;
        border-radius: none;
        font-weight: 700;
        cursor: pointer;
    }

    .content-card-unavailable-btn-next:hover {
        background-color: var(--black);
        color: var(--white);
    }
</style>