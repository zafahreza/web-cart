<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>
            History
            <strong>Orders</strong>
          </h2>
        </div>
      </div>

      <div class="mb-4">
        <div class="row mt-4" v-for="order in orders" :key="order.id">
          <CardOrder :order="order" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/NavbarComponent.vue";
import CardOrder from "@/components/CardOrderComponent.vue";
import axios from "axios";

export default {
  name: "OrderView",
  components: {
    Navbar,
    CardOrder,
  },
  data() {
    return {
      orders: [],
    };
  },
  methods: {
    setOrders(data) {
      this.orders = data;
      this.orders.forEach(order => {
        const date = new Date(order.created_at) 
        order.created_at = date.toLocaleString()
      });
    },
  },
  mounted() {
    let token = JSON.parse( localStorage.getItem('token') );
    if( token ){
      axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

      axios
      .get("http://localhost:3000/orders")
      .then((response) => this.setOrders(response.data.data.orders))
      .catch((error) => console.log(error));
  }

  },
};
</script>
<style>
</style>