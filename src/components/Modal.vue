<template>
  <section
    class="translucent-backdrop"
    v-if="this.modalStatus"
  >
    <div class="modal">
      <!-- TODO: This is kind of messy with multiple modal options.  -->
      <!-- Cosider making these separate components -->
      <!-- Problem is that you would have to emit modaStatus from each -->
      <!-- Also would have to emit the submitted data two levels up -->
      <div
        v-if="this.modalStatus === 'NUMBER_OF_PLAYERS'"
        class="modal-content"
      >
        <h3>How many are playing?</h3>
        <p>Enter a number from 2-6</p>
        <input
          type="text"
          inputmode="decimal"
          pattern="[2-6]*"
          v-model="numberOfPlayers"
          autofocus
        />
        <button
          type="button"
          @click="handleNextClick"
        >Next</button>
      </div>
      <div
        v-else-if="this.modalStatus === 'PLAYER_NAMES'"
        class="modal-content"
      >
        <div
          v-for="n in this.numberOfPlayers"
          :key="n"
        >
          <h3>Enter player {{ currentPlayerNumber }}'s name</h3>
          <input
            type="text"
            v-model="currentPlayerName"
            autofocus
            ref="nameInput"
          />
          <h3>Choose a color</h3>
          <div class="color-picker">
            <div
              v-for="(availableColor) in this.availableColors"
              class="color-square"
              :key="availableColor"
              :style="{ backgroundColor: availableColor, border: `${selectedColor === availableColor ? '1px solid yellow' : ''}` }"
              @click="this.handleColorSquareClick(availableColor)"
            ></div>
          </div>

          <button
            type="button"
            @click="handleNextClick"
          >Next</button>
        </div>
        <span
          v-if="this.errorStatus"
          class="error"
        >{{ this.errorStatus }}</span>
      </div>
    </div>
  </section>
</template>

<script>
import { ERROR_STATUS, MODAL_STATUS, COLORS } from '../assets/Constants.js'
export default {
  name: 'Modal',
  data() {
    return {
      availableColors: [COLORS.red, COLORS.orange, COLORS.blue, COLORS.white, COLORS.brown, COLORS.green],
      currentPlayerName: '',
      currentPlayerNumber: 1,
      errorStatus: '',
      modalStatus: MODAL_STATUS.numberOfPlayers,
      numberOfPlayers: '',
      players: [],
      selectedColor: ''
    }
  },
  methods: {
    handleNextClick() {
      // if (this.modalStatus === MODAL_STATUS.numberOfPlayers) this.getNumberOfPlayers()
      // if (this.modalStatus === MODAL_STATUS.playerNames) this.getPlayerNamesAndColors()
      if (this.modalStatus === MODAL_STATUS.numberOfPlayers) {
        if (this.numberOfPlayers < 2 || this.numberOfPlayers > 6) {
          return this.errorStatus = ERROR_STATUS.numberOfPlayersError
        }
        if (this.errorStatus) this.errorStatus = ''
        // TODO: emit number? Maybe not, just use length of players, no? 
        return this.modalStatus = MODAL_STATUS.playerNames
      }

      if (this.modalStatus === MODAL_STATUS.playerNames) {
        if (!this.selectedColor) {
          return this.errorStatus = ERROR_STATUS.playerColorError
        }
        this.currentPlayerNumber++
        if (this.currentPlayerNumber > this.numberOfPlayers) {
          this.modalStatus = ''
          this.$emit('set-players', this.players)
        }
        if (this.currentPlayerName === '') {
          this.currentPlayerName = `Player ${this.currentPlayerNumber - 1}`
        }
        this.players.push({ name: this.currentPlayerName, color: this.selectedColor })
        this.availableColors.splice(this.availableColors.indexOf(this.selectedColor), 1)
        this.currentPlayerName = ''
        this.selectedColor = ''
        this.errorStatus = ''
      }
    },
    // getNumberOfPlayers() {
    //   if (this.numberOfPlayers < 2 || this.numberOfPlayers > 6) {
    //     return this.errorStatus = ERROR_STATUS.numberOfPlayersError
    //   }
    //   if (this.errorStatus) this.errorStatus = ''
    //   // emit number
    //   return this.modalStatus = MODAL_STATUS.playerNames
    // },
    // getPlayerNamesAndColors() {
    //   this.currentPlayerNumber++
    //   if (this.currentPlayerNumber > this.numberOfPlayers) {
    //     this.modalStatus = ''
    //     // return emit playerNames
    //   }
    //   if (this.currentPlayerName === '') {
    //     this.currentPlayerName = `Player ${this.currentPlayerNumber}`
    //   }
    //   this.players.push({ name: this.currentPlayerName, color: this.selectedColor })
    //   this.availableColors.splice(this.availableColors.indexOf(this.selectedColor), 1)
    //   this.currentPlayerName = ''
    //   this.selectedColor = ''
    // },
    handleColorSquareClick(color) {
      this.selectedColor = color
    },
  },
  //TODO: Add focus to each input. 
  // Not sure if I'm using the ref wrong, or if I'm trying to access it before it's on the DOM
  // updated() {
  //   if (this.$refs.nameInput) this.$refs.nameInput.focus()
  // }

}

</script>

<style lang="scss" scoped>
@import "../scss/_variables.scss";

.translucent-backdrop {
  align-items: center;
  background: rgba(0, 0, 0, .5);
  backdrop-filter: blur(.75rem);
  display: flex;
  height: 100vh;
  justify-content: center;
  margin: 0;
  padding: 0;
  position: fixed;
  top: 0;
  width: 100vw;
  z-index: 5;
}

.modal {
  align-items: center;
  background: $white;
  border: 1px solid $black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-bottom: 20%;
  padding: 5rem 2rem;
  top: 0;
  width: 50%;
  z-index: 10;
}

.modal-content {
  width: 100%;
}

.color-picker {
  display: flex;
  justify-content: space-around;
}

.color-square {
  border-radius: 1rem;
  border: 1px solid $black;
  cursor: pointer;
  height: 5rem;
  width: 5rem;
}
</style>
