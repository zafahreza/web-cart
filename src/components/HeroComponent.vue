<template>
    <div class="hero">
      <!-- Desktop -->
      <div class="d-none d-md-block">
        <div class="row mt-4">
          <div class="col-md-6">
            <div class="d-flex h-100">
              <div class="justify-content-center align-self-center">
                <h2>
                  <strong>Need PC Component?,</strong>
                  <br />Buy Here
                </h2>
                <p>choose your PC Component on this website</p>
                <router-link class="btn btn-lg btn-success" to="/products">
                  <b-icon-arrow-right></b-icon-arrow-right> Order
                </router-link>
              </div>
            </div>
          </div>
          <div class="col-md-6">
            <img src="../assets/images/hero.png" width="100%" />
          </div>
        </div>
      </div>
  
     
    </div>
  </template>
  
  <script>
  export default {
    name: "HeroComponent",
  };
  </script>
  
  <style>
  </style>