<template>
  <div class="products-new">
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
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: ""
    };
  },
  created: function() {},
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
          this.$router.push('/')
        });
    }
  }
};
</script>