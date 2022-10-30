<template>
  <small>
    <span>{{ this.nextPlayer.name }}</span> is next
  </small>
</template>

<script>
export default {
  name: 'NameDisplayNext',
  props: ['nextPlayer']
}
</script>

<style lang="scss" scoped>
@import "../scss/_variables.scss";
</style>
