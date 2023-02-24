<template lang="html">
  <div class="app">
    <main>
      <SearchInput v-model="searchKeyword" @search="searchProducts"></SearchInput>
      <ul>
          <li v-for="product in products" :key="product.id" class="item flex" @click="moveToDetail(product.id)">
              <img :src="product.imageUrl" :alt="product.name" class="product-image"/>
              <span>{{ product.name }}</span>
              <span>{{ product.price }}$</span>
          </li>
      </ul>
    </main>
  </div>
</template>
<script>
import axios from 'axios';
import { fetchProductByKeyword } from '@/api/index';
import SearchInput from '~/components/SearchInput.vue';

export default {
    components: { SearchInput },

    async asyncData() {
        const response = await axios.get("http://localhost:3000/products");
        const data = response.data.map(item => {
            return {
                ...item,
                imageUrl: `${item.imageUrl}?random=${Math.random()}`
            };
        });
        return {
            products: data
        };
    },
    data() {
        return {
            products: [],
            searchKeyword: '',
        };
    },
    methods: {
        moveToDetail(id) {
          this.$router.push(`detail/${id}`);
        },

        async searchProducts() {
          const response = await fetchProductByKeyword(this.searchKeyword)
          this.products = response.data.map(item => {
              return {
                  ...item,
                  imageUrl: `${item.imageUrl}?random=${Math.random()}`
              };
          });
        }
    },
    
}
</script>
<style scoped>
.flex {
  display: flex;
  justify-content: center;
}
.item {
  display: inline-block;
  width: 400px;
  height: 300px;
  text-align: center;
  margin: 0 0.5rem;
  cursor: pointer;
}
.product-image {
  width: 400px;
  height: 250px;
}
.app {
  position: relative;
}
.cart-wrapper {
  position: sticky;
  float: right;
  bottom: 50px;
  right: 50px;
}
.cart-wrapper .btn {
  display: inline-block;
  height: 40px;
  font-size: 1rem;
  font-weight: 500;
}
</style>