<template>
    <div v-if="product">
        <img :src="require(`@/assets/${product.image}`)" alt="product.name" class="large-image">
        <h2>{{ product.name }}</h2>
        <p>Price: ${{ product.price }}</p>
        <p>{{ product.description }}</p>
        <router-link to="/">Back to Products</router-link>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            product: null
        };
    },
    methods: {
        fetchProduct() {
            const productId = this.$route.params.id;
            axios.get('/products.json').then(response => {
                this.product = response.data.find(p => p.id === parseInt(productId));
            });
        }
    },
    mounted() {
        this.fetchProduct();
    }
};
</script>

<style>
    .large-image {
        width: 100%;
        max-width: 600px;
        height: auto;
    }
</style>
