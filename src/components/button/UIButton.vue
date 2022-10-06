<template>
  <button
    :disabled="disabled || loading"
    class="button"
    :class="{ 'button--loading': loading }"
  >
    <slot v-if="!loading"></slot>
    <UILoader v-else></UILoader>
  </button>
</template>

<script setup>
import UILoader from "../loader/UILoader.vue";
defineProps({
  disabled: {
    type: Boolean,
    default: false,
    required: false,
  },
  loading: {
    type: Boolean,
    default: false,
    required: false,
  },
});
</script>

<style lang="scss">
@use "../../assets/styles/abstracts" as *;

.button {
  min-width: 10.5rem;
  padding: 1.4rem 2.4rem;
  background: $orange-bg-color;
  color: $white-color;
  border-radius: 4rem;
  border: none;
  font-family: "Gilroy", sans-serif;
  font-style: normal;
  font-weight: 700;
  font-size: 3rem;
  transition: 150ms background ease-in;

  @include media("<medium") {
    font-size: 2.2rem;
  }

  &:not(&--loading):not(:disabled) {
    cursor: pointer;
  }

  &:not(&--loading):not(:disabled):hover {
    background: $black-color;
  }

  &:not(&--loading):disabled {
    background: $btn-disabled-bg-color;
  }

  &:not(&--loading):not(:disabled):active {
    background: $gray-color;
  }
}
</style>