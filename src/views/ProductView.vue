<template>
    <div>
      <Navbar />
      <div class="container">
        <div class="row mt-4">
          <div class="col">
            <h2>
              List
              <strong>Products</strong>
            </h2>
          </div>
        </div>
  
        <div class="row mt-3">
          <div class="col">
            <div class="input-group mb-3">
              
              <input
                v-model="search"
                type="text"
                class="form-control"
                placeholder="Search products here..."
                aria-label="Search"
                aria-describedby="basic-addon1"
                @keyup="searchProduct"
              />
  
              <div class="input-group-prepend">
                <span class="input-group-text" id="basic-addon1">
                  <b-icon-search></b-icon-search>
                </span>
              </div>
            </div>
          </div>
        </div>
  
        <div class="row mb-4">
          <div class="col-md-4 mt-4" v-for="product in products" :key="product.id">
            <CardProduct :product="product" />
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  // @ is an alias to /src
  import Navbar from "@/components/NavbarComponent.vue";
  import CardProduct from "@/components/CardProduct.vue";
  import axios from "axios";
  
  export default {
    name: "ProductView",
    components: {
      Navbar,
      CardProduct,
    },
    data() {
      return {
        products: [],
        search: '',
      };
    },
    methods: {
      setProducts(data) {
        this.products = data;
      },
      searchProduct() {
        axios
        .get("http://localhost:3000/products?q="+this.search)
        .then((response) => this.setProducts(response.data.data.products))
        .catch((error) => console.log(error));
      }
    },
    mounted() {
      let token = JSON.parse( localStorage.getItem('token') );
    if( token ){
      axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

      axios
        .get("http://localhost:3000/products")
        .then((response) => this.setProducts(response.data.data.products))
        .catch((error) => console.log(error));
    }
  
    },
  };
  </script>
  <style>
  </style>