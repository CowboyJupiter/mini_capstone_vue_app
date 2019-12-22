

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
    Search <input type="text" v-model="searchTerm">
    <datalist id="titles">
        <option v-for="product in products">{{product.title}}</option>
    </div>


  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
import Vue2Filters from 'vue2-filters';
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
