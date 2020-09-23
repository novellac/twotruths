<template>
  <button
    class="px-12 py-4 mx-6 text-xl text-pink-900 duration-700 ease-out transform border-2 border-gray-400 border-solid rounded-lg bg-gradient-to-r from-gray-300 via-gray-300 to-gray-500"
    @click="sendGuess()"
    :disabled="selected"
    :class="{
      'shadow-lg hover:scale-125 border-pink-400 from-pink-300 via-pink-300 to-pink-500': !selected,
    }"
  >
    {{ fact.msg }}
    {{ fact.emoji }}
    {{ fact.id }}
    {{ selected }}
    <slot />
  </button>
</template>

<script>
export default {
  props: {
    fact: {
      type: Object,
      default: () => {},
    },
    selected: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    sendGuess() {
      this.$emit("calculate", this.fact.id);
    },
  },
};
</script>
