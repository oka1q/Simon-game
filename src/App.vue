<template>
  <div id="app">
    <h1>Игра Саймон говорит</h1>
    <GameBoard />
  </div>
</template>

<script>
import GameBoard from "./components/GameBoard";
export default {
  name: "App",
  components: {
    GameBoard,
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
}
</style>
