<template>
  <div class="home">

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

        <div>
          <h4>Edit Product</h4>
          <div>
            <div>
              Name: <input v-model="product.name">
            </div>

            <div>
              Price: <input v-model="product.price">
            </div>

            <div>
              Description: <input v-model="product.description">
            </div>

            <div>
              Image Url: <input v-model="product.image_url">
            </div>

            <button v-on:click="updateProduct(product)" class="btn btn-success">Update</button>
            <button v-on:click="destroyProduct(product)" class="btn btn-primary">Delete</button>
          </div>
        </div>
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
 