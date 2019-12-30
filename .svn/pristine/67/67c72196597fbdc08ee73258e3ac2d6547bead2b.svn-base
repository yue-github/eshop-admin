<template>
  <div class="pad" v-bind:style="{background: back, color: fore ? fore : '#fff'}">
    {{title}}
    <br>
    <span class="number">{{part1}}</span>
    <span v-if="part2 != null && part2.length > 0" class="number2"> / {{part2}}</span>
  </div>
</template>

<script>
module.exports = {
  name: 'bc-statbox',
  props: ['title', 'part1', 'part2', 'back', 'fore'],
}
</script>

<style scoped>
  .pad {
    text-align: center;
    border-radius: 3px;
    height: 66px;
    padding-top: 5px;
  }
  .pad > .number {
    font-family: Impact;
    font-size: 32px;
  }
  .pad > .number2 {
    font-family: Arial;
  }
</style>