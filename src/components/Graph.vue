<template>
  <p>Total Number of Rolls: {{ allRolls.length }}</p>
  <div class="graph">
    <div
      v-for="n in 11"
      :key="n + 1"
      class="graph-bar-holder"
    >
      <!-- NOTE: Below works using a template, which seems kind of like React.Fragment-->
      <!-- This could also be accomplished using a computed property [filter function] -->
      <!-- More info here: https://vuejs.org/style-guide/rules-essential.html#avoid-v-if-with-v-for -->
      <template v-for="(roll, index) in allRolls">
        <div
          v-if="roll === n + 1"
          :style="{ backgroundColor: colors[index % colors.length], height: `${100 / this.maxFrequency}%` }"
        ></div>
      </template>
    </div>
  </div>
  <div class="graph-number-boxes">
    <GraphNumberBox
      v-for="n in 11"
      :key="n + 1"
      :rollTotal="n + 1"
      :allRollsLength="allRolls.length"
      :currentCount="currentCount[n - 1]"
    />
  </div>
</template>

<script>
import GraphNumberBox from './GraphNumberBox.vue';
export default {
  name: 'Graph',
  components: { GraphNumberBox },
  props: ['allRolls', 'colors', 'maxFrequency', 'currentCount'],
  computed: {
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
  display: flex;
  flex-direction: column-reverse;
  width: 7rem;
}

.graph-number-boxes {
  display: flex;
  justify-content: space-around;
  margin: 2.5rem 0rem 0rem .1rem;
}
</style>
