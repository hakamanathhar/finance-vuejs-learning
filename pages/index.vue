<template>
<Login v-if="!isAuthenticated"/>
<Landing v-else/>
</template>

<script>
import { mapGetters } from 'vuex'
import Login from '~/pages/login'
import Landing from '~/pages/landing'
export default {
  components: {
    Login,
    Landing
  },
  name: 'IndexPage',
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
}
</script>
