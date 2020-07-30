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
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

</style>
