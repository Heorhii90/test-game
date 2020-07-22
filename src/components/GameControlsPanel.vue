<template>
  <div class="game-screen__game-controls">
    <select v-model="gameMode" :disabled="gameIsStarted">
      <option disabled value>Select game mode</option>
      <option v-for="(setting, key) in gameSettings" :key="setting.id">{{ key }}</option>
    </select>
    <input v-model="playerName" type="text" :disabled="gameIsStarted" />
    <button
      :disabled="gameIsStarted || isDisabled"
      @click="setGameValues"
    >{{ gameStatusOvered ? 'Play again' : 'Play' }}</button>
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
  }
</style>
