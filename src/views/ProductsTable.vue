<template>
  <div class="products-index">
    <h1>All Products</h1>
    <div>
      Filter Title: <input v-model="titleFilter" list='titles'>

      <datalist id="titles">
        <option v-for="product in products">{{ product.name }}</option>
      </datalist>
    </div>

    <table style="width:100%">
      <tr>
        <th v-on:click="setSortAttribute('id')">ID
          <span v-if="sortAttribute === 'id' && sortOrder === 1">&#8595;</span>
          <span v-else="sortAttribute === 'id' && sortOrder === -1">&#8593;</span></th>
        <th v-on:click="setSortAttribute('name')">Name<span v-if="sortAttribute === 'name'">&#8595;</span></th> 
        <th v-on:click="setSortAttribute('price')">Price<span v-if="sortAttribute === 'price'">&#8595;</span></th> 
      </tr>

      <tr v-for="product in orderBy(filterBy(products, titleFilter, 'name'), sortAttribute, sortOrder)">
        <td>{{ product.id }}</td>

        <router-link v-bind:to="'/products/' + product.id">
          <td>{{ product.name }}</td> 
        </router-link>
        <td>{{ product.formatted.price }}</td>
      </tr>

    </table>
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
      titleFilter: '',
      sortAttribute: 'title',
      sortOrder: 1
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => {
        this.products = response.data
    });
  },
  methods: {
    setSortAttribute: function(inputAttribute) {
      if (this.sortAttribute === inputAttribute) {
        this.sortOrder *= -1;
      } else {
        this.sortAttribute = inputAttribute;
        this.sortOrder = 1;
      }

    }
  },
  mixins: [Vue2Filters.mixin]
};
</script>




