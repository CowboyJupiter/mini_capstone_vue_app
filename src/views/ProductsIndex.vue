

<template>
  <div class="products-index">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <p>id: {{ product.id }}</p>
      <p>name: {{ product.name }}</p>
      <p>description: {{ product.description }}</p>
      <p>price: {{product.price }}</p>
      <p>image: {{product.image_url }}</p>
      <router-link v-bind:to="`/products/${product.id}`">show more</router-link>

      <hr>


    </div>


  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Index!",
      products: []
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.response = response.data;

    });
  },
  methods: {}
};
</script>
