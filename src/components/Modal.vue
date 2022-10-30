<template>
  <div
    v-if="this.modalStatus"
    class="modal"
  >
    <div v-if="this.modalStatus === 'NUMBER_OF_PLAYERS'">
      <h3>How many are playing?</h3>
      <p>Enter a number from 2-6</p>
      <input
        type="text"
        inputmode="decimal"
        pattern="[2-6]*"
        v-model="numberOfPlayers"
      />
      <button
        type="button"
        @click="handleNextClick"
      >Next</button>
    </div>
    <div v-else-if="this.modalStatus === 'PLAYER_NAMES'">
      <div
        v-for="n in this.numberOfPlayers"
        :key="n"
      >
        <h3>Enter player {{ n }}'s name</h3>
        <input
          type="text"
          v-model="playerName"
        />
      </div>
    </div>
    <span
      v-if="this.errorStatus"
      class="error"
    >{{ this.errorStatus }}</span>
  </div>
</template>

<script>
import { ERROR_STATUS, MODAL_STATUS } from '../assets/Constants.js'
export default {
  name: 'Modal',
  data() {
    return {
      errorStatus: '',
      modalStatus: MODAL_STATUS.numberOfPlayers,
      numberOfPlayers: ''
    }
  },
  methods: {
    handleNextClick() {
      if (this.modalStatus === MODAL_STATUS.numberOfPlayers) {
        if (this.numberOfPlayers >= 2 || this.numberOfPlayers <= 6) {
          if (this.errorStatus) this.errorStatus = ''
          // emit number
          return this.modalStatus = MODAL_STATUS.playerNames
        }
        else {
          this.errorStatus = ERROR_STATUS.numberOfPlayersError
        }
      }
      if (this.modalStatus === MODAL_STATUS.playerNames) {

      }
    }

    // emit player names
  }
}
</script>

<style lang="scss" scoped>
@import "../scss/_variables.scss";

.modal {
  z-index: 10;
  border: 1px solid $black;
}
</style>
