<template>
  <div class="game-screen__game-field">
    <Spinner v-if="fetchProcess" />
  </div>
</template>

<script>
import Spinner from "./Spinner";
import axios from "axios";

export default {
  name: "GameField",
  components: {
    Spinner
  }
};
</script>

<style lang="scss" scoped>
</style>