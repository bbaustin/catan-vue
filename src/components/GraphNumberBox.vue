<template>
  <div class="graph-number-box">
    <p>{{ rollTotal }}</p>
  </div>
</template>
<script>
export default {
  props: ['rollTotal']
}
</script>

<style lang="scss" scoped>
@import "../scss/_variables.scss";

.graph-number-box {
  align-items: center;
  border: .1rem solid $black;
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 7rem;
}
</style>
