<template>
  <div id="app">
    <hello-world />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
};
</script>

<style>
@import url(./assets/fonts/icomoon.css);
#app {
  width: 100%;
  margin: 0 auto;
  min-height: 780px;
  background: url("./assets/bg.png") no-repeat center 0 / cover;
}
</style>
