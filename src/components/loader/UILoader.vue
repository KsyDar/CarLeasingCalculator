<template>
    <span class="loader" :style="styleObject"></span>
  </template>
  
  <script setup>
  import {reactive} from "vue";
  
  const props = defineProps({
    size: {
      type: Number,
      default: 21,
      required: false
    }
  })
  
  const styleObject = reactive({
    width: `${props.size}px`,
    height:  `${props.size}px`
  })
  </script>
  
  <style lang="scss">
  .loader {
    border: 2px solid #FFF;
    border-bottom-color: transparent;
    border-radius: 50%;
    display: inline-block;
    box-sizing: border-box;
    animation: rotation 1s linear infinite;
  }
  
  @keyframes rotation {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  </style>