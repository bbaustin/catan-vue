<template>
  <p>Total Number of Rolls: {{ allRolls.length }}</p>
  <div class="graph">
    <div
      v-for="n in 11"
      :key="n + 1"
      class="graph-bar-holder"
    >
      <!-- TODO: Below is working, but I wonder if a computed property would be better. Because this div kind of serves no purpose, and is being added 0px to all column -->
      <div v-for="(roll, index) in allRolls">
        <div
          v-if="roll === n + 1"
          :style="{ backgroundColor: colors[index % colors.length] }"
        ></div>
      </div>
    </div>
  </div>
  <div class="graph-number-boxes">
    <GraphNumberBox
      v-for="n in 11"
      :key="n + 1"
      :rollTotal="n + 1"
    />
  </div>
</template>

<script>
import GraphNumberBox from './GraphNumberBox.vue';
export default {
  name: 'Graph',
  components: { GraphNumberBox },
  props: ['allRolls', 'colors'],
  computed: {
    filterRollsByTotal() {

    },
    determineColor(rollNumber) {
      for (let i = this.colors.length - 1; i < 0; i--) {
        if (rollNumber % i === 0) {
          return this.colors[i]
        }
      }
    }
  }
}
</script>

<style lang='scss' scoped>
@import "../scss/variables.scss";

.graph {
  border-bottom: .1rem solid $black;
  border-left: .1rem solid $black;
  display: flex;
  height: 12rem;
  justify-content: space-around;
}

.graph-bar-holder {
  border: .1rem dotted $black;
  border-bottom: none;
  display: flex;
  flex-direction: column-reverse;
  width: 7rem;
}

.graph-bar-holder div div {
  align-self: baseline;
  width: 100%;
  height: 1rem;
  border: 1px solid $black;
}

.graph-number-boxes {
  display: flex;
  justify-content: space-around;
  margin: 2.5rem 0rem 0rem .1rem;
}
</style>
