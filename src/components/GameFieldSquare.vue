<template>
  <div class="game-screen__game-square">
    <div
      v-for="gameField in gameSettingsSelected.fieldData"
      :key="gameField.id"
      :class="['game-screen__game-square__section', 
               gameField.status === 'isActive' && 'game-screen__game-square__section--active',
               gameField.status === 'player' && 'game-screen__game-square__section--player', 
               gameField.status === 'computer' && 'game-screen__game-square__section--computer']"
      :style="{width: `calc(400px / ${gameSettingsSelected.field})`}"
      @click="gameField.status === 'isActive' && handleClick(gameField.id)"
    />
  </div>
</template>

<script>
export default {
  name: "GameFieldSquare",
  props: {
    gameSettingsSelected: {
      type: Object,
      default() {
        return {};
      }
    },
    setActiveSquare: {
      type: Function,
      default() {
        return {};
      }
    },
    setUserClick: {
      type: Function,
      default() {
        return {};
      }
    },
    setComputerClick: {
      type: Function,
      default() {
        return {};
      }
    },
    setWinner: {
      type: Function,
      default() {
        return {};
      }
    },
    clearActiveSquare: {
      type: Function,
      default() {
        return {};
      }
    },
    gameIsStarted: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      gamePoints: null,
      actualId: null
    };
  },
  watch: {
  },
  beforeDestroy() {
    clearInterval(this.setInterval);
  },
  methods: {
    handleClick(id) {
      this.setUserClick(id);
      this.gamePoints.user += 1;
      this.actualId = null;
      this.restartInterval();
    }
  }
}
</script>

<style lang="scss" scoped>
</style>