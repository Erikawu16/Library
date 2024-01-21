<template>
  <div class="homepage">
    <img alt="Vue logo" src="../assets/logo.jpg">
    <homepage msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import homepage from '@/components/homepage.vue'

export default {
  name: 'HomeView',
  components: {
    homepage
  }
}
</script>
