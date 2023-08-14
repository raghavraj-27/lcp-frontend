<template>
  <NavbarCompo />
  <FilterCompo />
  <!-- <TableCompo /> -->
  <FooterCompo />
</template>

<script>
import FilterCompo from './components/FilterCompo.vue'
// import TableCompo from './components/TableCompo.vue'
import NavbarCompo from './components/NavBar.vue'
import FooterCompo from './components/FooterCompo.vue'

export default {
  name: 'App',
  components: {
    FilterCompo,
    NavbarCompo,
    FooterCompo,
    // TableCompo,
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
