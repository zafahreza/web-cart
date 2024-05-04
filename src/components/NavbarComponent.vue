<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <b-navbar-brand href="/">WebCart</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/products">Product</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/orders">History</router-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/cart">
                Cart
                <b-icon-bag></b-icon-bag>
                <span
                  class="badge badge-success ml-2"
                >{{ updateCart ? updateCart : total_count }}</span>
              </router-link>
            </li>
            <li class="nav-item">
              <button class="btn nav-link" @click="logout">
                Logout
              </button>
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "NavbarComponent",
  data() {
    return {
      total_count: 0,
    };
  },
  props: ["updateCart"],
  methods: {
    setTotal(data) {
      this.total_count = data;
    },
    logout(){
      localStorage.removeItem('token')
      axios.defaults.headers.common['Authorization'] = '';
      this.$router.push({ path: "/login"})
    },
  },
  mounted() {
    let token = JSON.parse( localStorage.getItem('token') );
    
    if( token ){
      axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

      axios
      .get("http://localhost:3000/carts")
      .then((response) => this.setTotal(response.data.data.count))
      .catch((error) => console.log(error));

    }else{
      this.$router.push({ path: "/login"})
    }
    
  },
};
</script>

<style>

</style>