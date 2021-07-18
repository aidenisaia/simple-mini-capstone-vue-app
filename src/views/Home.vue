/* eslint-disable prettier/prettier */
<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      Name:
      <input type="text" v-model="newproductparams.name" />
    </p>
    <p>
      Description:
      <input type="text" v-model="newproductparams.description" />
    </p>
    <p>
      Price:
      <input type="text" v-model="newproductparams.price" />
    </p>
    <p>
      Image:
      <input type="text" v-model="newproductparams.image_url" />
    </p>
    <button v-on:click="createProducts()">Add new recipe</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p>Name: {{ product.name }}</p>
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <img v-bind:src="product.image_url" />
      <hr />
    </div>
  </div>
</template>
<style>
img {
  width: 250px;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newproductparams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
      });
    },
    createProducts: function () {
      var productparams = {
        name: this.newproductparams.name,
        price: this.newproductparams.price,
        description: this.newproductparams.description,
        image_url: this.newproductparams.image_url,
      };
      axios.post("http://localhost:3000/products", productparams).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
        this.newproductparams = {};
      });
    },
  },
};
</script>
