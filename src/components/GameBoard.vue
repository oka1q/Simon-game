<template >
  <div class="main">
    <BoardButton v-for="i in 4" :key="'item-' + i" />
  </div>
</template>

<script>
import BoardItem from "./BoardItem.vue";
export default {
  name: "BoardItem",
  props: {},
  components: {
    BoardItem,
  },
};
</script>

<style scoped>
.main {
  background-color: rgba(182, 182, 182, 0.445);
  width: 400px;
  height: 400px;
  box-shadow: 0 0 15px 1px rgb(196, 196, 196);
  margin: 0 auto;
}
</style>
