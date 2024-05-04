<template>
    <div class="card shadow card-product">
      <img :src=" 'assets/images/' + product.picture " class="card-img-top" alt="..." />
      <div class="card-body">
        <h5 class="card-title">{{ product.name }}</h5>
        <p
          class="card-text"
        >price : Rp. {{ product.price }}</p>
        <router-link class="btn btn-success" :to="'/products/'+product.id"><b-icon-cart></b-icon-cart> Order</router-link>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "CardProduct",
    props: ["product"],
  };
  </script>
  
  <style>
  </style>