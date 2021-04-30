<template>
<div id="footer">
  <div class="container-fluid text-muted">
    <span>&copy;2021 <a href="https://github.com/Elmanzalawy/Covid-Tracker" target="_blank" class="text-muted">Mohamed Elmanzalawy</a></span>
  </div>
</div>
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
#footer{
  background: #1e1e1e !important;
  position: absolute;
  text-align: center;
  padding: 1rem 0;
  bottom: 0;
  width: 100%;
}


</style>
