<template>
  <section
    class="translucent-backdrop"
    v-if="this.modalStatus"
  >
    <div class="modal">
      <div
        v-if="this.modalStatus === 'NUMBER_OF_PLAYERS'"
        class="modal-content"
      >
        <p class="topic">How many are playing?</p>
        <input
          type="text"
          inputmode="decimal"
          pattern="[2-6]*"
          v-model="numberOfPlayers"
          autofocus
        />
        <p class="detail">Enter a number from 2-6</p>
        <button
          type="button"
          @click="handleNextClick"
        >
          Next
        </button>
      </div>
      <div
        v-else-if="this.modalStatus === 'PLAYER_NAMES'"
        class="modal-content"
      >
        <div
          v-for="n in this.numberOfPlayers"
          :key="n"
          style="width: 100%"
        >
          <p class="topic">Enter Player {{ currentPlayerNumber }}'s name</p>
          <input
            type="text"
            v-model="currentPlayerName"
            autofocus
            ref="nameInput"
          />
          <p
            class="topic"
            style="margin-top: 3rem"
          >
            Choose a color
          </p>
          <div class="color-picker">
            <div
              v-for="availableColor in this.availableColors"
              class="color-square-holder"
              id="availableColor"
              :key="availableColor"
              :style="{
                borderBottom: `${selectedColor === availableColor ? '.25rem solid black'}`,
              }"
              @click="this.handleColorSquareClick(availableColor)"
            >
              <label
                for="availableColor"
                class="hidden"
                >{{ availableColor }}</label
              >
              <input
                type="radio"
                name="color"
                class="color-square"
                :style="{ backgroundColor: availableColor }"
              />
            </div>
          </div>
          <button
            type="button"
            @click="handleNextClick"
          >
            Next
          </button>
        </div>
      </div>
      <p class="note">
        Note: Once the game begins, <br />
        hit 'm' to toggle dark/light mode.
      </p>
      <span
        v-if="this.errorStatus"
        class="error"
        >{{ this.errorStatus }}</span
      >
    </div>
  </section>
</template>

<script>
import { ERROR_STATUS, MODAL_STATUS, COLORS } from '../assets/Constants.js';
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
      selectedColor: '',
    };
  },
  methods: {
    handleNextClick() {
      // FIRST MODAL
      if (this.modalStatus === MODAL_STATUS.numberOfPlayers) {
        if (!/^([2-6]{1,})$/.test(this.numberOfPlayers)) {
          return (this.errorStatus = ERROR_STATUS.numberOfPlayersError);
        }
        if (this.errorStatus) this.errorStatus = '';
        this.modalStatus = MODAL_STATUS.playerNames;
        this.selectedColor = this.availableColors[0]; // Set first color radio as "selected"
        return;
      }
      // SECOND, THIRD, ETC... MODAL
      if (this.modalStatus === MODAL_STATUS.playerNames) {
        if (!this.selectedColor) {
          return (this.errorStatus = ERROR_STATUS.playerColorError);
        }
        this.currentPlayerNumber++;
        // Finished with modal
        if (this.currentPlayerNumber > this.numberOfPlayers) {
          this.modalStatus = '';
          this.$emit('set-players', this.players);
        }
        if (this.currentPlayerName === '') {
          this.currentPlayerName = `Player ${this.currentPlayerNumber - 1}`;
        }
        this.resetForNextPlayerNamesModal();
      }
    },
    resetForNextPlayerNamesModal() {
      this.players.push({ name: this.currentPlayerName, color: this.selectedColor });
      this.availableColors.splice(this.availableColors.indexOf(this.selectedColor), 1);
      this.currentPlayerName = '';
      this.selectedColor = this.availableColors[0];
      this.$refs.nameInput[0].focus();
      console.log(this.$refs.nameInput[0]);
      this.errorStatus = '';
    },
    handleColorSquareClick(color) {
      this.selectedColor = color;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../scss/modules/_colors.scss';

.translucent-backdrop {
  align-items: center;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(0.75rem);
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
  background: $ui-white;
  border: 1px solid $black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-bottom: 20%;
  padding: 5rem 7rem;
  top: 0;
  width: 50%;
  z-index: 10;
}

.modal-content {
  align-items: center;
  display: flex;
  flex-direction: column;
  text-align: center;
  width: 100%;
}

.color-picker {
  display: flex;
  justify-content: space-around;
  margin-bottom: 3rem;
}

input[type='radio'] {
  /* https://moderncss.dev/pure-css-custom-styled-radio-buttons/ /*
  /* Add if not using autoprefixer */
  -webkit-appearance: none;
  appearance: none;
  /* For iOS < 15 to remove gradient background */
  background-color: #fff;
  /* Not removed via appearance */
  margin: 0;
  padding: 0;
}

.color-square-holder {
  height: 6rem;
  border-bottom: 0.25rem solid transparent;
}

.color-square-holder:first-child {
  border-bottom: 0.25rem solid black;
}

.color-square-holder:hover {
  border-bottom: 0.25rem solid grey;
}

.color-square {
  border: 1px solid $black;
  cursor: pointer;
  height: 5rem;
  width: 5rem;
}

.hidden {
  position: absolute;
  overflow: hidden;
  clip: rect(0 0 0 0);
  height: 1px;
  width: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
}

p {
  margin: 0;
}

input {
  font-size: 2.4rem;
  height: 4rem;
  margin-bottom: 0.5rem;
  padding-left: 1rem;
}

input[inputmode='decimal'] {
  padding-right: 1rem;
  text-align: center;
  width: 3rem;
}

button {
  cursor: pointer;
  font-size: 2.4rem;
  height: 5rem;
  width: 10rem;
}

.topic {
  font-size: 3.6rem;
  margin-bottom: 1.5rem;
}

.detail {
  font-size: 1.6rem;
  margin-bottom: 2rem;
}

.note {
  font-size: 1.2rem;
  margin-top: 2rem;
  text-align: center;
}

@media screen and (max-width: 768px) {
  .modal {
    padding: 5rem 2rem;
    width: 75%;
  }

  .topic {
    font-size: 3rem;
  }

  .detail {
    font-size: 2rem;
    margin-top: 0;
  }

  .note {
    font-size: 1.5rem;
    margin-top: 1rem;
  }

  button {
    margin-bottom: 0;
  }

  //   .color-square-holder {
  //     height: 4rem;
  //   }
  //
  //   .color-square {
  //     height: 3rem;
  //     width: 3rem;
  //   }

  button {
    font-size: 2rem;
    height: 4rem;
    width: 8rem;
  }
}
</style>
