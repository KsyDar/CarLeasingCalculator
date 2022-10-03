<template>
  <button
      :disabled="disabled || loading"
      class="button"
      :class="{'button--loading': loading}">
    <slot v-if="!loading"></slot>
    <UILoader v-else></UILoader>
  </button>
</template>

<script setup>
import UILoader from "../Loader/UILoader.vue";
defineProps({
  disabled: {
    type: Boolean,
    default: false,
    required: false
  },
  loading: {
    type: Boolean,
    default: false,
    required: false
  }
})
</script>

<style lang="scss">
@use "UIButton";
</style>