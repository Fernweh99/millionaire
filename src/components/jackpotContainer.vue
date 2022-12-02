<template>
  <div class="prize-container">
    {{prize}} €
  </div>
</template>

<script>
export default {
  name: 'jackpotContainer',
  props: {
    prize: Number,
  }
}
</script>

<style>
  .prize-container {
    border: 1px solid white;
    color: white;
    padding: 5px 10px;
  }
</style>