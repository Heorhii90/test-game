<template>
  <div class="game-screen__result-table">
    <h2>Leader board</h2>
    <p
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

<style>

</style>
