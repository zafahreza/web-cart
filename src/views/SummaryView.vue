<template>
    <div class="cart">
      <Navbar />
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
                  <router-link to="/orders" class="text-dark">Orders</router-link>
                </li>
                <li class="breadcrumb-item active" aria-current="page">Summary</li>
              </ol>
            </nav>
          </div>
        </div>
  
        <div class="row">
          <div class="col mb-5">
            <h2>
              Order
              <strong>Summary</strong>
            </h2>
            <div class="row mt-4">
              <div class="col">
                <p>
                  Status: {{ order.status_confirmation }}
                </p>
              </div>
              <div class="col">
                <p class="float-right">
                  Transaction Date: {{ order.created_at }}
                </p>
              </div>
            </div>
            
            <div class="table-responsive mt-3">
              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">No</th>
                    <th scope="col">Picture</th>
                    <th scope="col">Product</th>
                    <th scope="col">Note</th>
                    <th scope="col">Product Count</th>
                    <th scope="col">Price</th>
                    <th scope="col">Total price</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(order_item, index) in order.order_items" :key="order_item.id">
                    <th>{{index+1}}</th>
                    <td>
                      <img
                        :src=" '../../assets/images/' + order_item.product.picture"
                        class="img-fluid shadow"
                        width="250"
                      />
                    </td>
                    <td>
                      <strong>{{ order_item.product.name }}</strong>
                    </td>
                    <td>{{ order_item.note ? order_item.note : "-" }}</td>
                    <td>{{ order_item.product_count }}</td>
                    <td align="right">Rp. {{ order_item.product.price }}</td>
                    <td align="right">
                      <strong>Rp. {{ order_item.product.price*order_item.product_count }}</strong>
                    </td>
                  </tr>
  
                  <tr>
                    <td colspan="6" align="right">
                      Total Before Discount :
                    </td>
                    <td align="right">
                      Rp. {{ order.total_before_discount }}
                    </td>
                    <td></td>
                  </tr>
                  <tr>
                    <td colspan="6" align="right">
                      Discount Vouncher :
                    </td>
                    <td align="right">
                      Rp. {{ order.discount }}
                    </td>
                    <td></td>
                  </tr>
                  <tr>
                    <td colspan="6" align="right">
                      <strong>Total After Discount :</strong>
                    </td>
                    <td align="right">
                      <strong>Rp. {{ order.total_after_discount }}</strong>
                    </td>
                    <td></td>
                  </tr>
                </tbody>
              </table>
            </div>

            <tr>
                    <td colspan="6" align="right">
                      Coupon Order Received :
                    </td>
                    <td align="right">
                      {{ order.order_coupon_received }}
                    </td>
                    <td></td>
                  </tr>
                  <tr>
                    <td colspan="6" align="right">
                      Coupon Product Received :
                    </td>
                    <td align="right">
                      {{ order.product_coupon_received }}
                    </td>
                    <td></td>
                  </tr>


          </div>
        </div>
  
        <!-- Form Checkout -->
        <div v-if="isDisplay" class="row justify-content-end mb-5">
          <div class="col-md-4">

            <button type="submit" class="btn btn-success float-right mt-3" @click="confirm">
                Confirm
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
    name: "SummaryView",
    components: {
      Navbar,
    },
    data() {
      return {
        request:{},
        order: {},
        isDisplay:false
      };
    },
    methods: {
      setOrder(data) {
        this.order = data;

        if (this.order.status_confirmation != 'Confirmed'){
          this.isDisplay = true
        }

        const date = new Date(this.order.created_at) 
        this.order.created_at = date.toLocaleString()
      },
      confirm() {
        this.request.status = "Confirmed"
        axios
        .patch("http://localhost:3000/orders/"+this.order.id, this.request)
        .then(() => {
              this.$router.push({ path: "/orders" });
              this.$toast.success("Order Confirmed", {
                type: "success",
                position: "top-right",
                duration: 3000,
                dismissible: true,
              });
            })
            .catch((err) => console.log(err));
      },
    },
    mounted() {
      let token = JSON.parse( localStorage.getItem('token') );
    if( token ){
      axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

      axios
        .get("http://localhost:3000/orders/"+ this.$route.params.id)
        .then((response) => this.setOrder(response.data.data))
        .catch((error) => console.log(error));
    }
    
    },
  };
  </script>
  
  <style>
  </style>