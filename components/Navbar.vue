<template>
<nav class="navbar navbar-expand-lg">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">YOUR FINANCE</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav ms-auto me-5">
              <li class="nav-item dropdown" v-if="isAuthenticated">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                  {{ loggedInUser.username }}
                </a>
                <ul class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
                  <li><nuxt-link class="dropdown-item" to="/profile">My Profile</nuxt-link></li>
                  <li><a class="dropdown-item" @click="logout">Logout</a></li>
                </ul>
              </li>
              <template v-else>
                <li class="nav-item">
                  <nuxt-link class="nav-link" to="/register">Register</nuxt-link>
                </li>
                <li class="nav-item">
                  <nuxt-link class="nav-link" to="/login">Log In</nuxt-link>
                </li>
              </template>
            </ul>
      </div>
  </div>
</nav>
</template>
<script>
import { mapGetters } from 'vuex'
export default {
  name: 'Navbar',
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  methods: {
    async logout() {
      await this.$auth.logout();
    },
  },
}
</script>
