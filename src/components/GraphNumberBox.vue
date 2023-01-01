<template>
  <div class="graph-number-box">
    <p>
      <span :style="{ color: this.highlight }">{{ rollTotal }}</span>
    </p>
    <p>{{ currentCount ? currentCount : 0 }}</p>
    <p>{{ allRollsLength ? ((currentCount / allRollsLength) * 100).toFixed(1) : 0 }}%</p>
  </div>
</template>
<script>
export default {
  props: ['rollTotal', 'currentCount', 'allRollsLength', 'highlight'],
};
</script>

<style lang="scss" scoped>
@import '../scss/_variables.scss';

.graph-number-box {
  align-items: center;
  border: 0.1rem solid $black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 7rem;
}

.dark .graph-number-box {
  border: 0.1rem solid $ui-white;
}

p {
  margin: 2rem 0 0 0;
}

p:first-child {
  font-size: 2.4rem;
  font-weight: 500;
  line-height: 2.4rem;
}

p:last-child {
  font-size: 1.8rem;
  margin-bottom: 2rem;
}
</style>
