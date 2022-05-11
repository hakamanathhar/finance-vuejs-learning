<template>
  <div class="alert alert-danger text-center">
    {{ message }}
  </div>
</template>
<script>
export default {
  name: 'Notification',
  props: ['message']
}
</script>
