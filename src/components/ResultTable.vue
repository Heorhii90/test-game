<template>
  <div class="game-screen__result-table">
    <h2 class="game-screen__title">Leader board</h2>
    <p
      class="game-screen__table-winners"
      v-for="winner in winners"
      :key="winner.id"
    >
      <span>{{ winner.winner }}</span>
      <span>{{ winner.date }}</span>
    </p>
  </div>
</template>

<script>
export default {
  name: 'ResultTable',
  props: {
    winners: {
      type: Array,
      default () {
        return []
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .game-screen {

    &__result-table {
      color: #999999;
      font-family: sans-serif;
    }

    &__table-winners {
      width: 300px;
      height: 50px;
      background: #cfd8dc;
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin: 5px 0;
      font-size: 18px;
      padding: 0 15px;
    }
  }
</style>
