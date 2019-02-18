<template>
  <div class="home">
    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.title }}</h2>
      <p>Description: {{product.description}}</p>
      <p>Image: {{ product.image_url}}</p>
      <p>Price: {{ product.price}}</p>
    </div>
  </div>
</template>

<script></script>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      products: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/products")
      .then(response => {
        this.products = response.data
    });
  },
  methods: {}
};
</script>
