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
    gamePoints: {
      deep: true,
      handler() {
        const { fieldData } = this.gameSettingsSelected;
        const { user, computer } = this.gamePoints;
        if (fieldData.length % 2 === 0) {
          if (user + 1 > fieldData.length / 2) {
            this.setGameWinner("user");
          }
          if (computer + 1 > fieldData.length / 2) {
            this.setGameWinner();
          }
        }
        if (user > fieldData.length / 2) {
          this.setGameWinner("user");
        }
        if (computer > fieldData.length / 2) {
          this.setGameWinner();
        }
      }
    },
    gameIsStarted: {
      deep: true,
      immediate: true,
      handler() {
        if (this.gameIsStarted) {
          (this.gamePoints = {
            user: 0,
            computer: 0
          }),
            this.getDefaultSquare();
        }
      }
    }
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
    },
    getRandomSquare() {
      const { fieldData } = this.gameSettingsSelected;
      const id = Math.floor(Math.random() * fieldData.length) + 1;
      if (fieldData.some(element => element.id === id && element.status))
        return this.getRandomSquare();
      return id;
    },
    getDefaultSquare() {
      this.setInterval = setInterval(() => {
        if (this.actualId) {
          this.setComputerClick(this.actualId);
          this.gamePoints.computer += 1;
        }
        const defaultId = this.getRandomSquare();
        this.actualId = defaultId;
        this.setActiveSquare(defaultId);
      }, this.gameSettingsSelected.delay);
    },
    restartInterval() {
      clearInterval(this.setInterval);
      this.getDefaultSquare();
    },
    setGameWinner(data) {
      this.clearActiveSquare();
      clearInterval(this.setInterval);
      this.setWinner(data);
    }
  }
};
</script>

<style lang="scss" scoped>
.game-screen__game-square {
  display: flex;
  width: 400px;
  flex-wrap: wrap;

  &__row {
    display: flex;
  }

  &__section {
    outline: 1px solid black;
    height: 80px;
    cursor: pointer;

    &--active {
      background: blue;
    }

    &--player {
      background: green;
    }

    &--computer {
      background: red;
    }
  }
}
</style>
