<template>
  <div>
    <div class="user-login-container">
      <div class="user--login">
        <div class="login--box">
          <div class="mb-4 text-center">
               <div class="text-center mb-4">
            <img src="@/assets/img/icon.svg" width="50" alt="">
        </div>
            <h2 class=" ">Admin Login</h2>
            <small>Enter details to login</small>
          </div>
          <form action="" @submit.prevent="login">
            <div class="mb-4">
              <label class="" for="">Email</label>
              <input type="email" v-model="credentials.email"/>
              <small
                class="text-danger"
                v-show="errorMsg"
                >* {{ errorMsg }}
              </small>
            </div>
            <div class="mb-4">
              <label class="" for="">Password</label>
              <input type="password" v-model="credentials.password"/>
              <small
                class="text-danger"
                v-show="errorMsg"
                >* {{ errorMsg }}
              </small>
            </div>
            <div>
              <div class="d-flex justify-content-center" v-if="loading">
                <div class="spinner-border" role="status">
                  <span class="sr-only">Loading...</span>
                </div>
              </div>
              <button type="submit" class="buy--btn" v-else>Login</button>
            </div>
          </form>

        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios'
export default {
  data(){
    return {
      loading: false,
      errorMsg: '',
      credentials:{
        email: '',
        password: ''
      },
    }
  },
    methods: {
        async login() {
      this.loading = true;
      try {
        let res = await axios.post("https://api.buybnb.io/api/auth/signin", this.credentials);
        const token = res.data.token
        const user = res.data.user
        this.$store.dispatch("login", { token, user });
        console.log(res);
        this.$toastify({
          text: `Welcome ${res.data.user.name}`,
          className: "info",
          style: {
            background: "green",
          },
        }).showToast();

          this.$router.push('/')
        
      } catch (error) {
        console.log(error.response.data.message);
        this.errorMsg = error.response.data.message;
      }
      this.loading = false;
      this.credentials = {}
    },
    }
}
</script>
