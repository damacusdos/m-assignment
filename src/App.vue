<template>
  <div id="app">
    <Header />
    <!-- <Search /> -->
    <Inventory />
  </div>
</template>

<script>
import Header from '@/components/header';
import Inventory from '@/components/inventory';

export default {
  name: 'App',
  components: {
    Header,
    Inventory,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500&display=swap');

body {
  margin: 0;
}
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
