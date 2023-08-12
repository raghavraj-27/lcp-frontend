<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <NavbarCompo />
  <TableCompo />
  <FooterCompo />
</template>

<script>
import TableCompo from './components/TableCompo.vue'
import NavbarCompo from './components/NavBar.vue'
import FooterCompo from './components/FooterCompo.vue'

export default {
  name: 'App',
  components: {
    TableCompo,
    NavbarCompo,
    FooterCompo,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
