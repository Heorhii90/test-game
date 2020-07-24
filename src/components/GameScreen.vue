<template>
  <div class="game-screen">
    <Spinner v-if="fetchProcess" />
    <div class="game-screen__container">
      <GameField :get-game-winners="getGameWinners" />
      <ResultTable :winners="winners" />
    </div>
  </div>
</template>

<script>
import Spinner from './Spinner';
import ResultTable from './ResultTable'
import GameField from './GameField'
import axios from 'axios';

export default {
  name: 'GameScreen',
  components: {
    Spinner,
    ResultTable,
    GameField
  },
  data() {
    return {
      fetchProcess: false,
      winners: []
    }
  },
  mounted () {
    this.getGameWinners()
  },
  methods: {
    getGameWinners () {
      this.fetchProcess = true,
      axios.get('https://starnavi-frontend-test-task.herokuapp.com/winners').then(response => {
        this.winners = response.data
        this.fetchProcess = false
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .game-screen {

    &__container {

      display: flex;
    }
  }
</style>
