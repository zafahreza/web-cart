<template>
    <div class="cart">
      <Navbar :updateCart="carts.length" />
      <div class="container mb-5">
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
                <li class="breadcrumb-item active" aria-current="page">Cart</li>
              </ol>
            </nav>
          </div>
        </div>
  
        <div class="row">
          <div class="col">
            <h2>
              My
              <strong>Cart</strong>
            </h2>
            <div class="table-responsive mt-3">
              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">#</th>
                    <th scope="col">Picture</th>
                    <th scope="col">Product</th>
                    <th scope="col">Note</th>
                    <th scope="col">Product Count</th>
                    <th scope="col">Price</th>
                    <th scope="col">Total price</th>
                    <th scope="col">Delete</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(cart, index) in carts" :key="cart.id">
                    <th>{{index+1}}</th>
                    <td>
                      <img
                        :src=" '../assets/images/' + cart.product.picture "
                        class="img-fluid shadow"
                        width="250"
                      />
                    </td>
                    <td>
                      <strong>{{ cart.product.name }}</strong>
                    </td>
                    <td>{{ cart.note ? cart.note : "-" }}</td>
                    <td>{{ cart.product_count }}</td>
                    <td align="right">Rp. {{ cart.product.price }}</td>
                    <td align="right">
                      <strong>Rp. {{ cart.product.price*cart.product_count }}</strong>
                    </td>
                    <td align="center" class="text-danger">
                      <b-icon-trash @click="deleteCart(cart.id)"></b-icon-trash>
                    </td>
                  </tr>
  
                  <tr>
                    <td colspan="6" align="right">
                      <strong>Total price :</strong>
                    </td>
                    <td align="right">
                      <strong>Rp. {{ totalprice }}</strong>
                    </td>
                    <td></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
  
        <!-- Form Checkout -->
        <div class="row justify-content-end">
          <div class="col-md-4">
            <b-form-select v-model="selected" :options="options"></b-form-select>

            <button type="submit" class="btn btn-success float-right mt-3" @click="checkout">
                <b-icon-cart></b-icon-cart>Order
              </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import Navbar from "@/components/NavbarComponent.vue";
  import axios from "axios";
  
  export default {
    name: "CartView",
    components: {
      Navbar,
    },
    data() {
      return {
        request:{},
        selected: null,
        cart_ids:[],
        carts: [],
        options:[
          {
            value: null,
            text: "please select a coupon"
          }
        ]
      };
    },
    methods: {
      setCoupon(data){
        data.forEach(d => {
          this.options.push({value: d.id, text: d.description})
        });
      },
      setCarts(data) {
        this.carts = data;
        this.carts.forEach(d => {
          this.cart_ids.push(d.id)
        });
      },
      deleteCart(id) {
        axios
          .delete("http://localhost:3000/carts/" + id)
          .then(() => {
            this.$toast.error("delete cart success", {
              type: "error",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
  
            axios
              .get("http://localhost:3000/carts")
              .then((response) => this.setCarts(response.data.data.carts))
              .catch((error) => console.log(error));
          })
          .catch((error) => console.log(error));
      },
      checkout() {
        if (this.cart_ids.length != 0) {
          this.request.coupon_id = this.selected;
          this.request.cart_ids = this.cart_ids
          axios
            .post("http://localhost:3000/orders", this.request)
            .then((response) => {
              this.$router.push({ path: "/orders/"+response.data.data.id });
              this.$toast.success("Order Success", {
                type: "success",
                position: "top-right",
                duration: 3000,
                dismissible: true,
              });
            })
            .catch((err) => console.log(err));
        } else {
          this.$toast.error("No Product Included", {
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
        .get("http://localhost:3000/carts")
        .then((response) => this.setCarts(response.data.data.carts))
        .catch((error) => console.log(error));

      axios
        .get("http://localhost:3000/coupons")
        .then((response) => this.setCoupon(response.data.data.coupons))
        .catch((error) => console.log(error))
    }
    
    },
    computed: {
      totalprice() {
        return this.carts.reduce(function (items, data) {
          return items + data.product.price * data.product_count;
        }, 0);
      },
    },
  };
  </script>
  
  <style>
  </style>