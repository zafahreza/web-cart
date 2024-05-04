<template>
    <div class="product-detail">
      <Navbar />
      <div class="container">
        <!-- breadcrumb -->
        <div class="row mt-4">
          <div class="col">
            <nav aria-label="breadcrumb">
              <ol class="breadcrumb">
                <li class="breadcrumb-item">
                  <router-link to="/" class="text-dark">Home</router-link>
                </li>
                <li class="breadcrumb-item">
                  <router-link to="/products" class="text-dark">Products</router-link>
                </li>
                <li class="breadcrumb-item active" aria-current="page">Product Order</li>
              </ol>
            </nav>
          </div>
        </div>
  
        <div class="row mt-3">
          <div class="col-md-6">
            <img :src=" '../assets/images/' + product.picture " class="img-fluid shadow" />
          </div>
          <div class="col-md-6">
            <h2>
              <strong>{{ product.name }}</strong>
            </h2>
            <hr />
            <h4>
              price :
              <strong>Rp. {{ product.price }}</strong>
            </h4>
            <form class="mt-4" v-on:submit.prevent>
              <div class="form-group">
                <label for="product_count">Product Count</label>
                <input type="number" class="form-control" v-model="request.product_count" />
              </div>
              <div class="form-group">
                <label for="note">Note</label>
                <textarea
                  v-model="request.note"
                  class="form-control"
                  placeholder="write your note here..."
                ></textarea>
              </div>
  
              <button type="submit" class="btn btn-success" @click="order">
                <b-icon-cart></b-icon-cart>Add to Cart
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import Navbar from "@/components/NavbarComponent.vue";
  import axios from "axios";
  
  export default {
    name: "ProductDetail",
    components: {
      Navbar,
    },
    data() {
      return {
        request: {},
        product: {},
      };
    },
    methods: {
      setProduct(data) {
        this.product = data;
      },
      order() {
        if (this.request.product_count) {
          this.request.product_id = this.product.id;
          this.request.product_count = parseInt(this.request.product_count)
          axios
            .post("http://localhost:3000/carts", this.request)
            .then(() => {
              this.$router.push({ path: "/cart"})
              this.$toast.success("add to card success", {
                type: "success",
                position: "top-right",
                duration: 3000,
                dismissible: true,
              });
            })
            .catch((err) => console.log(err));
        } else {
          this.$toast.error("product count is required", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
        }
      },
    },
    mounted() {
      let token = JSON.parse( localStorage.getItem('token') );
    if( token ){
      axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

    axios
      .get("http://localhost:3000/products/"+this.$route.params.id)
      .then((response) => this.setProduct(response.data.data))
      .catch((error) => console.log(error))
    }
    
    },
  };
  </script>
  
  <style>
  </style>