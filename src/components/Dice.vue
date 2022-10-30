<template>
  <section v-if="currentRoll.length">
    <div>
      <div class="dice">{{ currentRoll[0] }}</div>
      <div class="dice">{{ currentRoll[1] }}</div>
    </div>
    <p>Total: {{ this.currentTotal }}</p>
  </section>
  <h3 v-else>Click anywhere to roll!</h3>
</template>

<script>
export default {
  name: 'Dice',
  props: ['currentRoll', 'currentTotal']
}
</script>

<style lang='scss' scoped>
@import "../scss/_variables.scss";

section {
  align-items: center;
  display: flex;
  flex-direction: column;
}

section div {
  display: flex;
  flex-direction: row;
}

.dice {
  align-items: center;
  border: .1rem solid $black;
  display: flex;
  font-size: 5rem;
  height: 10rem;
  justify-content: center;
  margin: 1rem;
  width: 10rem;
}
</style>
