<template>
  <div class="home">
    <h1>New Product</h1>
    <div>
      <div>
        Name: <input v-model="newProductName">
      </div>

      <div>
        Price: <input v-model="newProductPrice">
      </div>

      <div>
        Description: <input v-model="newProductDescription">
      </div>

      <div>
        Image Url: <input v-model="newProductImageUrl">
      </div>

      <button v-on:click="createProduct()">Create</button>
    </div>

    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      <div>
        <button v-on:click="currentProduct = product">More Info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Description: {{ product.description }}</p>
        <p>Price: {{ product.formatted.price}}</p>
      </div>
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
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
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
    createProduct: function() {
      console.log("Create the product");
      var params = {
                   name: this.newProductName,
                   price: this.newProductPrice,
                   description: this.newProductDescription,
                   image_url: this.newProductImageUrl
                  };

      axios.post("/api/products", params)
        .then(response => {
          console.log("Success", response.data);
          this.products.push(response.data)
        });
    }
  }
};
</script>
 