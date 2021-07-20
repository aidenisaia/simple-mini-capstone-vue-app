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
      <button v-on:click="showProducts(product)">More Info</button>
      <p>Price: {{ product.price }}</p>
      <img v-bind:src="product.image_url" />
      <hr />
    </div>
    <dialog id="product-description">
      <form method="dialog">
        <p>
          Name:
          <input type="text" v-model="currentproduct.name" />
        </p>
        <p>
          price:
          <input type="text" v-model="currentproduct.price" />
        </p>
        <p>
          description:
          <input type="text" v-model="currentproduct.description" />
        </p>
        <p>
          image_url:
          <input type="text" v-model="currentproduct.image_url" />
        </p>

        <button v-on:click="updateProduct(currentproduct)">Update</button>
        <button v-on:click="destroyProduct(currentproduct)">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>
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
      currentproduct: {},
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
    showProducts: function (theproduct) {
      console.log(theproduct);
      this.currentproduct = theproduct;
      document.querySelector("#product-description").showModal();
    },
    updateProduct: function (theproduct) {
      console.log(theproduct);
      var editParams = theproduct;
      axios.patch("http://localhost:3000/products/" + editParams.id, editParams).then((response) => {
        console.log(response.data);
      });
    },
    destroyProduct: function (theProduct) {
      console.log("destroying..");
      axios.delete(`http://localhost:3000/products/${theProduct.id}`).then((response) => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>
