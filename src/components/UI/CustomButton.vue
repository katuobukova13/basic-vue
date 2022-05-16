<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'custom-button'
}
</script>

<style scoped>
.btn {
  background: none;
  color: #00bd7e;
  border: 2px solid #00bd7e;
  padding: 10px;
}
.btn:hover{
  cursor: pointer;
}
</style>