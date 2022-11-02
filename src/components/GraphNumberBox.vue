<template>
  <div class="graph-number-box">
    <p>{{ rollTotal }}</p>
    <p>{{ currentCount }}</p>
    <p>{{ (currentCount / allRollsLength * 100).toFixed(1) }}%</p>
  </div>
</template>
<script>
export default {
  props: ['rollTotal', 'currentCount', 'allRollsLength'],
}
</script>

<style lang="scss" scoped>
@import "../scss/_variables.scss";

.graph-number-box {
  align-items: center;
  border: .1rem solid $black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 7rem;
}
</style>
