<template>
  <div class="game-screen__game-field">
    <Spinner v-if="fetchProcess" />
    <GameControlsPanel
      :game-settings="gameSettings"
      :set-game-settings="setGameSettings"
      :game-status-overed="!!winnerName"
      :game-is-started="gameIsStarted"
    />
    <p v-if="winnerName">
      {{ winnerName }} won =)
    </p>
    <GameFieldSquare
      v-if="gameSettingsSelected"
      :game-settings-selected="gameSettingsSelected"
      :set-active-square="setActiveSquare"
      :set-user-click="setUserClick"
      :set-computer-click="setComputerClick"
      :clear-active-square="clearActiveSquare"
      :set-winner="setWinner"
      :game-is-started="gameIsStarted"
    />
  </div>
</template>

<script>
import GameControlsPanel from "./GameControlsPanel";
import GameFieldSquare from "./GameFieldSquare";
import Spinner from "./Spinner";
import axios from "axios";

export default {
  name: "GameField",
  components: {
    GameControlsPanel,
    GameFieldSquare,
    Spinner
  },
  props: {
    getGameWinners: {
      type: Function,
      default() {
        return {};
      }
    }
  },
  data() {
    return {
      fetchProcess: false,
      gameSettings: {},
      gameSettingsSelected: null,
      winnerName: null,
      userName: "",
      gameIsStarted: false
    };
  },
  mounted() {
    (this.fetchProcess = true),
      axios
        .get("https://starnavi-frontend-test-task.herokuapp.com/game-settings")
        .then(response => {
          this.gameSettings = response.data;
          this.fetchProcess = false;
        });
  },
  methods: {
    setGameSettings(data) {
      const gameValues = this.gameSettings[data.gameMode];
      const fieldData = Array(gameValues.field ** 2)
        .fill()
        .map((element, index) => {
          return {
            id: index + 1,
            status: ""
          };
        });
      this.gameSettingsSelected = { ...gameValues, fieldData };
      this.userName = data.playerName;
      this.gameIsStarted = true;
    },
    clearActiveSquare() {
      const { fieldData } = this.gameSettingsSelected;
      return fieldData.forEach(element => {
        if (element.status === "isActive") element.status = "";
      });
    },
    setActiveSquare(id) {
      const { fieldData } = this.gameSettingsSelected;
      return fieldData.forEach(element => {
        if (id === element.id) element.status = "isActive";
      });
    },
    setUserClick(id) {
      const { fieldData } = this.gameSettingsSelected;
      fieldData.forEach(element => {
        if (id === element.id) return (element.status = "player");
      });
    },
    setComputerClick(id) {
      const { fieldData } = this.gameSettingsSelected;
      fieldData.forEach(element => {
        if (id === element.id) element.status = "computer";
      });
    },
    setWinner(data) {
      this.gameIsStarted = false;
      this.winnerName = data ? this.userName : "computer";
      axios
        .post("https://starnavi-frontend-test-task.herokuapp.com/winners", {
          winner: data ? this.userName : "computer",
          date: new Date().toLocaleString()
        })
        .then(() => this.getGameWinners());
    }
  }
};
</script>

<style lang="scss" scoped>
.game-screen__game-field {
  display: flex;
  flex-direction: column;
}
</style>
