<template>
<nav id="navbar" class="navbar navbar-dark bg-dark">
  <span class="navbar-brand mb-0 h1">Covid Tracker</span>
</nav>
</template>

<script>
export default {
  name: 'Navbar',
  props: {
    // msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#navbar{
  background: #1e1e1e !important;
}
</style>
