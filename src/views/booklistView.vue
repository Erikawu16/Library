<template>
  <div class="booklist">
    <img alt="Vue logo" src="../assets/logo.jpg">
    <booklist msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import booklist from '@/components/booklist.vue'

export default {
  name: 'booklistView',
  components: {
    booklist
  }
}
</script>
