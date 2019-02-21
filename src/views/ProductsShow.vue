<template>
  <div class="product-show">
    <h1>{{ product.name }}</h1>
    <img v-bind:src="product.image_url" v-bind:alt="product.name">
    <h4>Price: {{ product.formatted.price }}</h4>
    <p>Description: {{ product.description }}</p>

    <router-link :to="'/products/' + product.id + '/edit'">Edit</router-link>
    <button v-on:click="destroyProduct()">Delete</button>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      product: {
        id: "",
        name: "",
        description: "",
        image_url: "",
        tax: "",
        total: "",
        formatted:{
          price: "",
          tax: "",
          total: ""}
      }
    };
  },
  created: function() {
    axios.get('/api/products/' + this.$route.params.id)
      .then(response => {
        console.log(response.data);
        this.product = response.data;
      });
  },
  methods: {
    destroyProduct: function() {
      axios.delete("/api/products/" + this.product.id)
        .then(response => {
          console.log("Success", response.data);
          this.$router.push('/');
        });
    }
  }
};
</script>