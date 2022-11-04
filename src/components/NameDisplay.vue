<template>
  <p class="display-name">
    <span :style="{ color: this.previousPlayer.color }">{{ this.previousPlayer ? this.previousPlayer.name : '' }}</span>
    just rolled
  </p>
  <p class="sub-name">
    Next it's <span :style="{ color: this.currentPlayer.color }">{{ this.currentPlayer ?
        this.currentPlayer.name : ''
    }}</span>'s turn
  </p>
</template>

<script>
export default {
  name: 'NameDisplay',
  props: ['currentPlayer', 'previousPlayer']
}
</script>
