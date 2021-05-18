<template>
  <Home/>
</template>

<script>
import Home from '@/components/Home.vue'

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style lang="scss">
#app {
  background: var(--bg-color);
  color: var(--first-color);
}
</style>
