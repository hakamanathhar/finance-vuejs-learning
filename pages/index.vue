<template>
<Login v-if="!isAuthenticated"/>
<Landing v-else/>
</template>

<script>
import { mapGetters } from 'vuex'
import Login from '~/pages/Login'
import Landing from '~/pages/Landing'
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
