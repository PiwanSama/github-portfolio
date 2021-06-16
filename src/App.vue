<template>
  <div id="app">
    <img alt="Github logo" src="./assets/logo.png" />

    <GithubTokenForm />

    <User />

    <Repos />
  </div>
</template>

<script>
import GithubTokenForm from "./components/GithubTokenForm.vue";
import Repos from "./components/Repos.vue";
import User from "./components/User.vue";

export default {
  name: "App",
  components: {
    GithubTokenForm,
    Repos,
    User,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
