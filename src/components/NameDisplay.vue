<template>
  <h2>
    <span :style="{ color: this.previousPlayer.color }">{{ this.previousPlayer ? this.previousPlayer.name : '' }}</span>
    just rolled
  </h2>
  <h2>
    Now it's <span :style="{ color: this.currentPlayer.color }">{{ this.currentPlayer ? this.currentPlayer.name : ''
    }}</span>'s turn
  </h2>
</template>

<script>
export default {
  name: 'NameDisplay',
  props: ['currentPlayer', 'previousPlayer']
}
</script>

<style lang="scss" scoped>
@import "../scss/_variables.scss";

h2 {
  font-size: 4.2rem;
  line-height: 3.6rem;
  text-align: center;
}
</style>
