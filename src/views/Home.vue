<template>
  <div class="home">

    <h1>All Products</h1>
    <div v-for="product in products">
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

export default {
  data: function() {
    return {
      products: [],
      currentProduct: {}
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => {
        this.products = response.data
    });
  },
  methods: {
    updateProduct: function(inputProduct) {
      var params = {
                    name: inputProduct.name,
                    price: inputProduct.price,
                    description: inputProduct.description,
                    image_url: inputProduct.image_url
                   };

      axios.patch("/api/products/" + inputProduct.id, params)
        .then(response => {
          console.log("Success", response.data)
          inputProduct = response.data;
        });
    },
    destroyProduct: function(inputProduct) {
      axios.delete("/api/products/" + inputProduct.id)
        .then(response => {
          console.log("Success", response.data);
          var index = this.products.indexOf(inputProduct);
          this.products.splice(index, 1);
        });
    }
  }
};
</script>
 