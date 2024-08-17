<template>
  <div id="app">
    <CategoriesTree />
  </div>
</template>

<script>
import CategoriesTree from "./components/CategoriesTree.vue"

export default {
  name: 'App',
  components: {
    CategoriesTree
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
