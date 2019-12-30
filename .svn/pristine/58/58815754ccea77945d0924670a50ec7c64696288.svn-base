<template>
  <div class="panel">
    <div class="header">
      <i :class="icon ? icon : 'el-icon-search'"></i> {{title}}
    </div>
    <div class="body">
      <slot></slot>
    </div>
    <div class="footer">
      <slot name="footer"></slot>
    </div>
  </div>
</template>

<script>
module.exports = {
  name: 'bc-panel',
  props: ['title', 'icon'],
}
</script>

<style scoped>
  .panel {
    padding: 20px;
    background: white;
    border-radius: 3px;
  }
  .panel > .header {
    height: 40px;
    margin-bottom: 20px;
    border-bottom: 1px solid #E5E9F2;
  }
  .panel > .footer {
    padding-top: 20px;
    border-top: 1px solid #E5E9F2;
    margin-top: 20px;
  }
</style>