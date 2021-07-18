/* eslint-disable prettier/prettier */
/* eslint-disable prettier/prettier */
<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <hr />
    <div v-for="product in products" v-bind:key="product.id">
      <p>Name: {{ product.name }}</p>
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <hr />
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      console.log("In indexProducts");
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
      });
    },
  },
};
</script>
