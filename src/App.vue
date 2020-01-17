<template>
  <div class="container">
    <img alt="Vue logo" src="./assets/logo.png" />
    <Todolist />
  </div>
</template>

<script>
import Todolist from "./components/Todolist";

export default {
  name: "app",
  components: {
    Todolist
  }
};
</script>

<style scoped>
.container {
  width: 600px;
  margin: 0 auto;
  font-size: 24px;
}

img {
  display: block;
  margin: 20px auto;
  height: 75px;
}
</style>
