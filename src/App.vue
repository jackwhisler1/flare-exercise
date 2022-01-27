<template>
  <div id="app">
    <div class="bg-info" id="nav">
      <router-link to="/">Home</router-link>
      |
      <a href="https://github.com/jackwhisler1/flare-exercise">GitHub Repository</a>
    </div>
    <router-view />
    <Footer></Footer>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
}
</style>

<script>
import Footer from "./components/Footer";
export default {
  components: {
    Footer,
  },
};
</script>
