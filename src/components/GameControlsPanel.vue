<template>
  <div class="game-screen__game-controls">
    <select
      class="game-screen__game-controls__mode"
      v-model="gameMode"
      :disabled="gameIsStarted"
    >
      <option
        disabled 
        value
      >
        Select game mode
      </option>
      <option
        v-for="(setting, key) in gameSettings"
        :key="setting.id"
      >
        {{ key }}
      </option>
    </select>
    <input
      class="game-screen__game-controls__input-name"
      placeholder = "Enter your name"
      v-model="playerName"
      type="text"
      :disabled="gameIsStarted"
    >
    <button
      class="game-screen__game-controls__button-play"
      :disabled="gameIsStarted || isDisabled"
      @click="setGameValues"
    >
      {{ gameStatusOvered ? 'Play again' : 'Play' }}
    </button>
  </div>
</template>

<script>
export default {
  name: "GameControlsPanel",
  props: {
    gameSettings: {
      type: Object,
      default() {
        return {};
      }
    },
    setGameSettings: {
      type: Function,
      default() {
        return {};
      }
    },
    gameStatusOvered: {
      type: Boolean,
      default: false
    },
    gameIsStarted: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      gameMode: "",
      playerName: ""
    };
  },
  computed: {
    isDisabled() {
      const { gameMode, playerName } = this;
      if (gameMode && playerName) return false;
      return true;
    }
  },
  methods: {
    setGameValues() {
      const { gameMode, playerName } = this;
      this.setGameSettings({ gameMode, playerName });
    }
  }
};
</script>

<style lang="scss" scoped>
.game-screen__game-controls {
  display: flex;
  height: 50px;
  font-size: 18px;

  &__mode {
    background: #cfd8cf;
    border: 1px solid #cfd8cf;
    border-radius: 5px;
    width: 250px;
    font-size: 18px;
    padding-left: 10px;
  }

  &__input-name {
    background: #f3f3f3;
    border: 1px solid #f3f3f3;
    border-radius: 5px;
    padding-left: 10px;
    margin: 0 10px;
    font-size: 18px;
  }

  &__button-play {
    width: 100px;
    border: 1px solid #7b8d93;
    border-radius: 5px;
    background: #7b8d93;
    color: #f3f3f3;
    font-size: 18px;
    font-weight: 700;
  }
}
</style>
