

<template>
  <div class="home">
    <p>Product Name <input type="text" v-model="newProductName"></p>
    <p>Product Price <input type="text" v-model="newProductPrice"></p>
    <p>Product Description <input type="text" v-model="newProductDescription"></p>
    <p>Product Image <input type="text" v-model="newProductImage"></p>

    <transition-group appear enter-active-class="animate fadeIn" leave-active-class="animated fadeOut">
      



      
      <div v-for="product in filterBy(products, searchTerm, 'name')">    <!-- <v-bind:class="{selected: product.selected}> -->
      <input type="text" v-model="searchTerm">
      <button v-on:click="setSortAttribute('name')"> sort by name</button>
      <button v-on:click="('price')"> sort by price </button>
      <button v-on:click="(product)">select product</button>
     <!--  <div v-for="product in orderBy(products, searchTerm, 'name', 'price')"> -->
        <p>id: {{ product.id }}</p>
        <p>title: {{ product.name }}</p>
        <p>description:{{ product.description }} </p>
        <img v-bind:src="product.image_url" v-bind:alt="product.name">
        <br>

        <button v-on:click="toggleInfo(product)">More Info</button>
        <button v-on:click="addProduct(product)"> Add a new product</button>
        <div v-if="currentProduct">
          <p>description: {{product.description }} </p>


          <p>name: <input type="text" v-model="product.name"></p>
          <p>price: <input type="text" v-model="product.price"></p>
          <p>description: <input type="text" v-model="product.description"></p>
          <p>image_url: <input type="text" v-model="product.image_url"></p>
        </div>
      </div>
     
    </transition-group>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
import Vue2Filters from 'vue2-filters';
import "animate.css/animate.css";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      searchTerm: '',
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImage: "",
      currentProduct: {},
    };
  },

  created: function() {
    console.log('i am created');
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    addProduct: function() {
      console.log('adding product..');
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription, image: this.newProductImage
      };
      axios.post("/api/products", params).then(response => { 
        console.log(response.data);
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
        this.newProductImage = "";
      });

    },
    toggleInfo: function(theProduct) {
      console.log(theProduct);
      if (this.currentProduct === theProduct) {
        this.currentProduct = null;
      } else {
        this.currentProduct = theProduct;
      }
      console.log('in toggle');

    },
    updateProduct: function(theProduct) {
      console.log('updating the product');
      console.log(theProduct);

      var params = {
        name: theProduct.name,
        price: theProduct.price,
        descrition: theProduct.description,
        image_url: theProduct.image_url,
      };

      axios.patch(`/api/products/${theProduct.id}`, params).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.price = response.data.price;
        theProduct.description = response.data.description;
      });
    },
    setSortAttribute: function(theAttribute) {
      this.setSortAttribute = theAttribute;
    }


  }
};
</script>
