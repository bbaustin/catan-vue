<template>
  <h2>
    <span>{{ this.currentPlayer.name }}</span> just rolled
  </h2>
</template>

<script>
export default {
  name: 'NameDisplayCurrent',
  props: ['currentPlayer']
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
