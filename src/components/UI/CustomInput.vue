<template>
 <input :value="modelValue" @input="updateInput" type="text" class="input">
</template>

<script>
export default {
  name: "custom-input",
  props: {
    modelValue: [String, Number]
  },
  methods: {
    updateInput(event) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style scoped>
.input {
  border: 1px solid #00bd7e;
  padding: 15px;
  margin-top: 10px;
  width: 100%;
}
</style>