<template>
  <div id="app">
    <Cart />
  </div>
</template>

<script>
import Cart from "./components/Cart.vue";

export default {
  name: "App",
  components: {
    Cart,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
