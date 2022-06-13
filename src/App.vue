<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link :to="{ name: 'Products'}">Products</router-link> |
    <router-link :to="{ name: 'about' }">About</router-link>
    <i class="fas fa-shopping-cart fa-2x"></i>
  </nav>
  
  <router-view/>
</template>

<script>


export default {
  
}
</script>
<style >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #fff;
  background: #42b983;
  padding: 10px;
  border-radius: 5px;
}
.fa-shopping-cart{
  color: #42b983;
  float: right;
}
</style>
