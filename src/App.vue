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
    <!--TODO  Make colors better -->
    <Graph
      :allRolls="allRolls"
      :colors="['#3b8fd8', '#fb4d43', '#F7F7F7']"
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
export default {
  name: 'App',
  components: {
    Dice, Graph, Modal, NameDisplayCurrent, // TODO: NameDisplayNext
  },
  methods: {
    handleClick() {
      this.rollDice()
      this.advancePlayer()
    },
    rollDice() {
      this.currentRoll = [Math.floor(Math.random() * 6) + 1, Math.floor(Math.random() * 6) + 1]
      this.allRolls.push(this.currentRoll[0] + this.currentRoll[1])
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
          color: '#F7F7F7',
        },
        {
          name: 'Kaori',
          color: '#fb4d43'
        },
        {
          name: 'Sam',
          color: '#3b8fd8'
        }
      ], //TODO: Add this in modal. Doing graph first.
      currentRoll: [],
      allRolls: [],
      currentPlayerIndex: 0
    }
  }
}
</script>
