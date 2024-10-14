<template>
    <div class="wrap">
        <div>
            <select v-model="selectedFilter" @change="filterProducts">
                <option value="all">All</option>
                <option value="cheap">Cheap</option>
                <option value="medium">Medium</option>
                <option value="expensive">Expensive</option>
            </select>
        </div>

        <div class="product-list">
            <div v-for="product in filteredProducts" :key="product.id" class="product-card"
                @click="viewDetails(product.id)">
                <img :src="require(`@/assets/${product.image}`)" alt="product.name" class="product-image">
                <h3>{{ product.name }}</h3>
                <p>Price: ${{ product.price }}</p>
                <router-link :to="`/product/${product.id}`">View Details</router-link>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            products: [],
            filteredProducts: [],
            selectedFilter: 'all'
        };
    },
    methods: {
        fetchProducts() {
            axios.get('/products.json').then(response => {
                this.products = response.data;
                this.filteredProducts = this.products;
            });
        },
        filterProducts() {
            if (this.selectedFilter === 'all') {
                this.filteredProducts = this.products;
            } else if (this.selectedFilter === 'cheap') {
                this.filteredProducts = this.products.filter(product => product.price < 3);
            } else if (this.selectedFilter === 'medium') {
                this.filteredProducts = this.products.filter(product => product.price >= 3 && product.price <= 4);
            } else if (this.selectedFilter === 'expensive') {
                this.filteredProducts = this.products.filter(product => product.price > 4);
            }
        },
        viewDetails(id) {
            this.$router.push({ name: 'ProductDetails', params: { id } });
        }
    },
    mounted() {
        this.fetchProducts();
    }
};
</script>

<style>
.wrap,
.product-list {
    display: flex;
}
.wrap {
    justify-content: center;
}
.product-list {
    flex-wrap: wrap;
    justify-content: center;
}
.product-card {
    width: 200px;
    border: 1px solid #ccc;
    padding: 10px;
    margin: 10px;
    cursor: pointer;
    text-align: center;
}
.product-image {
    width: 100px;
    height: auto;
}
</style>
