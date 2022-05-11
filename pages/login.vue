<template>
  <section class="section">
    <div class="container">
      <div class="row">
        <div class="col-5 m-auto pt-5">
          <div class="card mt-5">
            <div class="card-body px-5">
              <h2 class="title text-center">Welcome back!</h2>
              <Notification :message="error" v-if="error"/>
              <form method="post" @submit.prevent="login">
                <div class="mb-3">
                  <label for="exampleInputEmail1" class="form-label">Username</label>
                  <input type="text"
                    name="username"
                    v-model="username"
                    class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label">Password</label>
                  <input
                    type="password"
                    name="password"
                    v-model="password"
                    class="form-control" id="exampleInputPassword1">
                </div>
                <button type="submit" class="w-100 btn btn-info">Login</button>
              </form>

              <div class="text-center" style="margin-top: 20px">
                <p>
                  Don't have an account? <nuxt-link to="/register">Register</nuxt-link>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>

import Notification from '~/components/Notification'
export default {
  name: 'Login',
  components: {
    Notification,
  },
  data() {
    return {
      username: '',
      password: '',
      error: null
    }
  },
  methods: {
    async login() {
      try {
        await this.$auth.loginWith('local', {
          data: {
            username: this.username,
            password: this.password
          }
        })
        this.$router.push('/')
      } catch (e) {
        this.error = e.response.data.message
      }
    }
  },
  middleware: 'guest'
}
</script>
