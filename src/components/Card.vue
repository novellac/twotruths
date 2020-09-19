<template>
  <button @click="$emit('calculate')">
    {{ message }}
    <slot />
  </button>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped></style>
