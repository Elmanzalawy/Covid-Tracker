<template>
  <div id="app">
    <Navbar/>
    <div id="content-wrapper" class="container">
        <div class="row">
          <div class="col-md-6 p-4">
            <h3>Global Cases</h3>
            <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Nobis mollitia ratione necessitatibus ullam ducimus eveniet repudiandae quia quasi eos quibusdam. Voluptates obcaecati eligendi maiores culpa, tempore iste repudiandae animi deserunt soluta voluptate modi sapiente velit. Adipisci magni tempora unde nemo, pariatur eum quisquam repudiandae asperiores in praesentium explicabo minima maxime!</p>
          </div>
          <div class="col-md-6 p-4">
            <h3>Cases by country</h3>
            <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Nobis mollitia ratione necessitatibus ullam ducimus eveniet repudiandae quia quasi eos quibusdam. Voluptates obcaecati eligendi maiores culpa, tempore iste repudiandae animi deserunt soluta voluptate modi sapiente velit. Adipisci magni tempora unde nemo, pariatur eum quisquam repudiandae asperiores in praesentium explicabo minima maxime!</p>
          </div>
        </div>

        <div class="row">
          <div class="col-md-4">
            <h4>Section #1</h4>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam ipsa consequuntur in recusandae non eum nesciunt, nisi maxime perspiciatis ratione. Quam voluptas asperiores velit optio id nobis reiciendis recusandae impedit rerum nostrum exercitationem, quia ea corrupti possimus eaque omnis, nam dolorem nesciunt ex voluptatibus ad sint nulla modi odio. Sunt!</p>
          </div>
          <div class="col-md-4">
            <h4>Section #2</h4>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam ipsa consequuntur in recusandae non eum nesciunt, nisi maxime perspiciatis ratione. Quam voluptas asperiores velit optio id nobis reiciendis recusandae impedit rerum nostrum exercitationem, quia ea corrupti possimus eaque omnis, nam dolorem nesciunt ex voluptatibus ad sint nulla modi odio. Sunt!</p>
          </div>
          <div class="col-md-4">
            <h4>Section #3</h4>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam ipsa consequuntur in recusandae non eum nesciunt, nisi maxime perspiciatis ratione. Quam voluptas asperiores velit optio id nobis reiciendis recusandae impedit rerum nostrum exercitationem, quia ea corrupti possimus eaque omnis, nam dolorem nesciunt ex voluptatibus ad sint nulla modi odio. Sunt!</p>
          </div>
        </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    Footer
  }
}
</script>

<style>
#app {
  min-height: 100vh;
  position: relative;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
  background: #383838;
}

</style>
