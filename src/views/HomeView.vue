<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />

      <div class="row mt-4">
        <div class="col">
          <h2>
            Best
            <strong>Product</strong>
          </h2>
        </div>
        <div class="col">
          <router-link to="/products" class="btn btn-success float-right">
            <b-icon-eye></b-icon-eye> Explore
          </router-link>
        </div>
      </div>

      <div class="row mb-4">
        <div class="col-md-4 mt-4" v-for="product in products" :key="product.id">
          <CardProduct :product="product"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/NavbarComponent.vue";
import Hero from "@/components/HeroComponent.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {
    Navbar,
    Hero,
    CardProduct,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
  },
  mounted() {
    let token = JSON.parse( localStorage.getItem('token') );
    
    if( token ){
      axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

    axios
      .get("http://localhost:3000/products?limit=3")
      .then((response) => this.setProducts(response.data.data.products))
      .catch((error) => console.log(error))
    }
  },
};
</script>