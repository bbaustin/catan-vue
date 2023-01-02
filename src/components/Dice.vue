<template>
  <section>
    <div>
      <div class="dice">{{ currentRoll[0] }}</div>
      <div class="dice">{{ currentRoll[1] }}</div>
    </div>
    <p>
      Total: <span>{{ this.currentTotal }}</span>
    </p>
  </section>
</template>

<script>
export default {
  name: 'Dice',
  props: ['currentRoll', 'currentTotal'],
};
</script>

<style lang="scss" scoped>
@import '../scss/modules/_colors.scss';

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
  border: 0.1rem solid $black;
  display: flex;
  font-size: 5rem;
  height: 10rem;
  justify-content: center;
  margin: 1rem;
  width: 10rem;
}

.dark .dice {
  border: 0.1rem solid $ui-white;
}

p {
  margin: 1rem;
}
</style>
