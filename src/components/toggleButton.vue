<script setup>
const props = defineProps({
  label: String,
  active: {
    type: Boolean,
    default: false,
  },
});
</script>

<template>
  <button
    :class="{
      active: active,
    }"
  >
    {{ label }}
  </button>
</template>

<style scoped>
button {
  padding-left: 0.75rem;
  padding-right: 0.75rem;
  padding-top: 0.25rem;
  padding-bottom: 0.25rem;
  border-radius: 0.5rem;
  border: 1px solid white;
}

button.active {
  background-color: #5470af;
  color: white;
}
</style>
