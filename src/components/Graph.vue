<template>
  <section>
    <p>
      Total Number of Rolls:
      <span :style="{ fontWeight: 700 }">{{ allRolls.length }}</span>
    </p>
    <div class="graph">
      <div v-for="n in 11" :key="n + 1" class="graph-bar-holder">
        <!-- NOTE: Below works using a template, which seems kind of like React.Fragment-->
        <!-- This could also be accomplished using a computed property [filter function] -->
        <!-- More info here: https://vuejs.org/style-guide/rules-essential.html#avoid-v-if-with-v-for -->
        <template v-for="(roll, index) in allRolls">
          <div
            v-if="roll === n + 1"
            class="graph-bar-part"
            :style="{
              backgroundColor: colors[index % colors.length],
              height: `${100 / this.maxFrequency}%`,
            }"
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
        :highlight="allRolls.at(-1) === n + 1 ? this.highlight : ''"
      />
    </div>
  </section>
</template>

<script>
import GraphNumberBox from './GraphNumberBox.vue';
export default {
  name: 'Graph',
  components: { GraphNumberBox },
  props: ['allRolls', 'colors', 'maxFrequency', 'currentCount', 'highlight'],
  methods: {
    filterRolls() {
      allRolls;
    },
  },
  data() {
    return {
      filteredAllRolls: this.allRolls,
    };
  },
};
</script>

<style lang="scss" scoped>
@import '../scss/modules/_colors.scss';

section {
  margin: 0 2rem;
}

.graph {
  border-bottom: 0.1rem solid $black;
  border-left: 0.1rem solid $black;
  display: flex;
  height: 12rem;
  justify-content: space-around;
}

.dark .graph {
  border-bottom: 0.1rem solid $ui-white;
  border-left: 0.1rem solid $ui-white;
}

.graph-bar-holder {
  display: flex;
  flex-direction: column-reverse;
  width: 7rem;
}

.graph-bar-part {
  border: 1px solid rgba(0, 0, 0.1);
  border-bottom: none;
}

.graph-number-boxes {
  display: flex;
  justify-content: space-around;
  margin: 2.5rem 0rem 0rem 0.1rem;
}
@media screen and (max-width: 768px) {
  .graph {
    height: 50rem;
  }
  .graph-bar-part {
    border: none;
  }
}
</style>
