<template>
<Login v-if="!isAuthenticated"/>
<Landing v-else/>
</template>

<script>
import { mapGetters } from 'vuex'
import Login from './Login'
import Landing from './Landing'
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
