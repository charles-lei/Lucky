<template>
  <div id="app">
<!--     <a href="https://www.mytokenpocket.vip">
      <img class="logo" src="./assets/logo.png" alt>
    </a> -->
    <lucky/>
  </div>
</template>

<script>
import lucky from "./components/lucky";

export default {
  name: "App",
  components: {
    lucky
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.logo {
  height: 50px;
}
</style>
