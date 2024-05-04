<template>
    <div class="container">
        <div class=" card shadow card-login w-50 mx-auto mt-5">
        <b-form v-on:submit.prevent @reset="onReset" v-if="show">
        
        <b-form-group
          id="name"
          label="Name:"
          label-for="name"
        >
        <b-form-input
            id="name"
            v-model="form.name"
            placeholder="Enter fullname"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="username"
          label="Username:"
          label-for="username"
        >
        <b-form-input
            id="username"
            v-model="form.username"
            placeholder="Enter username"
            required
          ></b-form-input>
        </b-form-group>
  
        <b-form-group id="password" label="Password:" label-for="password">
          <b-form-input
            id="password"
            v-model="form.password"
            type="password"
            placeholder="Enter password"
            required
          ></b-form-input>
        </b-form-group>
  
        <b-button type="submit" variant="primary" @click="login" class="btn btn-success">SignUp</b-button>
        <b-button type="reset" variant="danger" class="ml-2">Reset</b-button>
      </b-form>
    </div>
    </div>
    
  </template>
  
  <script>
import axios from 'axios'
    export default {
        name: "SignUpView",
      data() {
        return {
          form: {
            name:'',
            username: '',
            password: '',
          },
          show: true
        }
      },
      methods: {
        login(event){
          axios
          .post("http://localhost:3000/users", this.form)
          .then(() => {
            this.$toast.success("SignUp Success", {
                type: "success",
                position: "top-right",
                duration: 3000,
                dismissible: true,
              });
            this.$router.push({ path: "/"})
          })
          
          .catch(() => {
            this.$toast.error("invalid data input", {
              type: "error",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          });

          event.preventDefault()
        },
        onReset(event) {
          event.preventDefault()
          // Reset our form values
          this.form.username = ''
          this.form.password = ''
          // Trick to reset/clear native browser form validation state
          this.show = false
          this.$nextTick(() => {
            this.show = true
          })
        }
      },
      mounted() {
      let token = JSON.parse( localStorage.getItem('token') );
    
      if( token ){
        axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

        this.$router.push({ path: "/"})
      }
  },
    }
  </script>
<style>

</style>