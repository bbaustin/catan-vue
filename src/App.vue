<template>
  <Modal v-if="!players.length" @set-players="(players) => setPlayers(players)" />
  <main @click="handleClick">
    <NameDisplay
      v-if="allRolls.length"
      :currentPlayer="players[currentPlayerIndex]"
      :previousPlayer="players[previousPlayerIndex]"
    />
    <div v-else>
      <p v-if="this.players.length" class="display-name">
        <span :style="{ color: this.players[0].color }">{{ this.players[0].name }}</span>
        is up first!
      </p>
      <p class="sub-name">Click anywhere to roll!</p>
    </div>
    <Dice :currentRoll="currentRoll" :currentTotal="allRolls.at(-1)" />
    <!--TODO:  Send colors differnelty -->
    <Graph
      :allRolls="allRolls"
      :colors="Object.values(players).map((player) => player.color)"
      :maxFrequency="maxFrequency"
      :currentCount="currentCount"
      :highlight="players.length ? players[previousPlayerIndex].color : ''"
    />
    <!-- <NameDisplayNext :nextPlayer="players[function to get correct index even if at end of array] TODO: -->
  </main>
</template>

<script>
import Dice from "./components/Dice.vue";
import Graph from "./components/Graph.vue";
import Modal from "./components/Modal.vue";
import NameDisplay from "./components/NameDisplay.vue";
export default {
  name: "App",
  components: {
    Dice,
    Graph,
    Modal,
    NameDisplay,
  },
  methods: {
    handleClick() {
      this.rollDice();
      this.countRolls();
      this.calculateFrequency();
      this.advancePlayer();
    },
    rollDice() {
      this.currentRoll = [
        Math.floor(Math.random() * 6) + 1,
        Math.floor(Math.random() * 6) + 1,
      ];
      this.allRolls.push(this.currentRoll[0] + this.currentRoll[1]);
    },
    countRolls() {
      this.currentCount = [];
      for (let i = 2; i < 13; i++) {
        //TODO: Improve this? More like simple counter, haha
        this.currentCount.push(this.allRolls.filter((roll) => roll == i).length);
      }
    },
    calculateFrequency() {
      let currentMax = Math.max(...this.currentCount);
      if (this.maxFrequency < currentMax) {
        this.maxFrequency = currentMax;
      }
    },
    advancePlayer() {
      this.previousPlayerIndex = this.currentPlayerIndex;
      if (this.currentPlayerIndex === this.players.length - 1) {
        return (this.currentPlayerIndex = 0);
      }
      return this.currentPlayerIndex++;
    },
    setPlayers(players) {
      this.players = players;
    },
  },
  data() {
    return {
      players: [],
      currentRoll: [],
      allRolls: [],
      previousPlayerIndex: 0,
      currentPlayerIndex: 0,
      maxFrequency: 1,
      currentCount: [],
    };
  },
};
</script>
