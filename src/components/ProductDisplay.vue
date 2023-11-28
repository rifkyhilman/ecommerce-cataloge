<template>
    <div :class="{'container bg-blue':isMen, 'container bg-pink':isWomen, 'container bg-gray':isOther}">
        <div class="overlay">
            <img src="../assets/images/bg-pattern.svg" />
        </div>
        <div class="container-card">
            <div class="image-card">
                <img :src="image" />
            </div>
            <div class="content-card">
                <div class="content-card-top">
                    <div class="content-card-title">
                        <h3 class="font-navy">
                            {{ title }}
                        </h3>
                    </div>
                    <div class="content-card-rating">
                        <div class="categeori">
                            <span> {{ category }} </span>
                        </div>
                        <div class="content-card-rating-detail">
                            <span> {{ rating }}/5 </span>
                            <div class="content-card-rating-icon">
                                <span class="bg-navy circle"></span>
                                <span class="bg-navy circle"></span>
                                <span class="bg-navy circle"></span>
                                <span class="bg-navy circle"></span>
                                <span class="bg-navy circle"></span>
                            </div>
                        </div>
                    </div>
                    <div class="content-card-description">
                    <p> {{ description }} </p> 
                    </div>
                </div>
                <div class="content-card-bottom">
                    <div class="content-card-price">
                        <span class="font-navy">${{ price }}</span>
                    </div>
                    <div class="content-card-btn">
                        <button class="content-card-btn-buy">Buy Now</button>
                        <button class="content-card-btn-next" @click="Tambah">Next Product</button>
                    </div>
                </div>
            </div> 
        </div>
        <UnavailablePage v-if="isOther" />
    </div>
</template>

<script>
import UnavailablePage from './UnavailablePage.vue';


export default {
    name: "ProductDisplay",
    components: {
        UnavailablePage
    },
    data() {
        return {
            index: 1,
            title: null,
            price: null,
            description: null,
            image: null,
            rating: null,
            category: null,
            isMen: null,
            isWomen: null,
            isOther: null,
            searchDebounce: null,
            loading:false,
            womens: "women's clothing",
            mens: "men's clothing"
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
            clearTimeout(this.searchDebounce);
            this.searchDebounce = setTimeout(() => {
                this.fetchFromApi(val);
            }, 2000);
        },
        async fetchFromApi(val) {
            try {
                const apiUrl = process.env.VUE_APP_API_URL;
                const api = await fetch(apiUrl + val);
                const productData = await api.json();

                this.loading = false;
                this.title = productData.title;
                this.price = productData.price;
                this.description = productData.description;
                this.image = productData.image;
                this.rating = productData.rating.rate;
                this.category = productData.category;

                if (productData.category === "men's clothing"){
                    this.isMen = true;
                    this.isWomen = false;
                    this.isOther = false;
                } else if (productData.category === "women's clothing"){
                    this.isWomen = true;
                    this.isMen = false;
                    this.isOther = false;
                } else {
                    this.isOther = true;
                    this.isMen = false;
                    this.isWomen = false;
                }

                setTimeout(() => {
                    this.$emit('sendDataFromChild', false);
                }, 3000);
                    
            } catch (error) {
                console.log(error);
            }
        
        },
        Tambah() {
            this.index == 20 ? this.index = 1 : this.index++;
            this.loading = true;
        }
    }
}

</script>

<style>

    .overlay {
        width: 100%;
        position: absolute;
        z-index: 1;
        top: 0;
        left: 0;
    }

    .overlay img {
        width: 100%;
        height: 450px;
    }


    .container-card {
        display: grid;
        grid-template-columns: 30% auto;
        gap: 16px;
        width: 80%;
        height: 70vh;
        z-index: 2;
        background-color: var(--white);
        border-radius: 10px;
        padding: 48px 32px;
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
        margin-top: 14px;
        font-size: 16px;
        color: var(--dark-gray);
        padding-bottom: 16px;
        border-bottom: 1px solid var(--gray);
    }

    .content-card-rating-detail {
        display: grid;
        grid-template-columns: auto auto;
        gap: 5px;
        margin-right: 25px;
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
        background-color: var(--navy);
        color: var(--white);
        border: none;
        width: 35%;
        height: 36px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
    }

    .content-card-btn-buy:hover {
        background-color: var(--white);
        color: var(--navy);
        border: 2px solid var(--navy);
    }

    .content-card-btn-next {
        background-color: var(--white);
        color: var(--navy);
        border: none;
        border: 2px solid var(--navy);
        width: 35%;
        height: 36px;
        font-size: 16px;
        font-weight: 700;
        cursor: pointer;
        margin-left: 12px;
    }

    .content-card-btn-next:hover {
        background-color: var(--navy);
        color: var(--white);
    }
</style>