<template>
  <div id="app">
    <Game />
  </div>
</template>

<script>
import Game from "./components/Game";

export default {
  name: "App",
  components: {
    Game
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
}
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background: url("assets/table.jpg");
  height: 100vh;
  background-size: cover;
  margin: 0;
  padding: 0;
}
</style>
