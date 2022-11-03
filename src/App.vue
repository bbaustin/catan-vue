<template>
  <Modal />
  <div
    id="app"
    @click="handleClick"
  >
    <NameDisplayCurrent :currentPlayer="players[currentPlayerIndex]" />
    <Dice
      :currentRoll="currentRoll"
      :currentTotal="allRolls.at(-1)"
    />
    <!--TODO:  Send colors differnelty -->
    <Graph
      :allRolls="allRolls"
      :colors="['#3b8fd8', '#fb4d43', '#D2D2D2', '#fab913', '#AC6D00', '#2ECC40']"
      :maxFrequency="maxFrequency"
      :currentCount="currentCount"
    />
    <!-- <NameDisplayNext :nextPlayer="players[function to get correct index even if at end of array] TODO: -->
  </div>
</template>

<script>
import Dice from './components/Dice.vue'
import Graph from './components/Graph.vue'
import Modal from './components/Modal.vue'
import NameDisplayCurrent from './components/NameDisplayCurrent.vue'
// import NameDisplayNext from './components/NameDisplayNext.vue'  // TODO:
import { COLORS } from './assets/Constants'
export default {
  name: 'App',
  components: {
    Dice, Graph, Modal, NameDisplayCurrent, // TODO: NameDisplayNext
  },
  methods: {
    handleClick() {
      this.rollDice()
      this.countRolls()
      this.calculateFrequency()
      this.advancePlayer()
    },
    rollDice() {
      this.currentRoll = [Math.floor(Math.random() * 6) + 1, Math.floor(Math.random() * 6) + 1]
      this.allRolls.push(this.currentRoll[0] + this.currentRoll[1])
    },
    countRolls() {
      this.currentCount = []
      for (let i = 2; i < 13; i++) { //TODO: Improve this? More like simple counter, haha
        this.currentCount.push(this.allRolls.filter((roll) => roll == i).length);
      }
    },
    calculateFrequency() {
      let currentMax = Math.max(...this.currentCount)
      if (this.maxFrequency < currentMax) {
        this.maxFrequency = currentMax
      }
    },
    advancePlayer() {
      if (this.currentPlayerIndex === this.players.length - 1) {
        return this.currentPlayerIndex = 0
      }
      return this.currentPlayerIndex++
    }
  },
  data() {
    return {
      players: [
        // TODO: It would be cool, but not entirely necessary, to send these colors via scss somehow
        {
          name: 'Ben',
          color: '#D2D2D2',
        },
        {
          name: 'Kaori',
          color: '#fb4d43'
        },
        {
          name: 'Sam',
          color: '#3b8fd8'
        },
        {
          name: 'Phil',
          color: '#fab913'
        },
        {
          name: 'Kyoha',
          color: '#AC6D00'
        },
        {
          name: 'Hikari',
          color: '#2ECC40'
        }
      ], //TODO: Add this in modal. Doing graph first.
      currentRoll: [],
      allRolls: [],
      currentPlayerIndex: 0,
      maxFrequency: 1,
      currentCount: []
    }
  }
}
</script>
