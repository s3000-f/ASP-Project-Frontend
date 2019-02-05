<template>
  <HelloWorld />
</template>

<script>
import HelloWorld from '../components/HelloWorld'

export default {
  components: {
    HelloWorld
  },
  created () {
    if (this.$store.state.user == null) {
      this.$router.push('/')
    }
  }
}
</script>
