<template>
  <div class="home">
    <div>
      Filter Title: <input v-model="titleFilter">
    </div>

    <h1>All Products</h1>
    <div v-for="product in filterBy(products, titleFilter, 'name')">
      <h2>{{ product.name }}</h2>
      <router-link v-bind:to="'/products/' + product.id">
        <img v-bind:src="product.image_url" v-bind:alt="product.name">
      </router-link>
    </div>
  </div>
</template>

<style>
  img {
    width: 250px;
    border-style: solid;
    border-width: 10px;
  }
</style>

<script>
var axios = require('axios');
import Vue2Filters from 'vue2-filters';

export default {
  data: function() {
    return {
      products: [],
      currentProduct: {},
      titleFilter: ''
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => {
        this.products = response.data
    });
  },
  methods: {},
  mixins: [Vue2Filters.mixin]
};
</script>
 