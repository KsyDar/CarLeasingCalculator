<template>
  <div class="slider" :class="{ sliderDisabled: disabled }">
    <div class="slider__label">
      {{ label }}
    </div>
    <div class="slider__text-wrapper" tabindex="0">
      <slot
        name="input"
        :value="modelValue"
        :customized-value="customizedValue"
        :spacedValue="spacedValue"
      >
        <input
          type="text"
          v-model="customizedValue"
          @change="inputChange($event.target.value)"
          @focus="customizedValue = customValue(customizedValue)"
          @blur="customizedValue = spacedValue(customizedValue)"
          class="slider__input-text"
          :disabled="disabled"
        />
      </slot>
      <div class="slider__input-mark">
        {{ afterInput }}
      </div>
    </div>
    <div class="slider__range-wrapper">
      <input
        type="range"
        :min="min"
        :max="max"
        v-model="modelValue"
        class="slider__input-range"
        :style="{ backgroundSize: ProgressLineSize + '%' }"
        @input="sliderChange($event.target.value)"
        :disabled="disabled"
      />
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "@vue/runtime-core";

const props = defineProps({
  modelValue: {
    type: Number,
    default: 0,
    required: false,
  },
  label: {
    type: String,
    default: "",
    required: false,
  },
  afterInput: {
    type: String,
    default: "",
    required: false,
  },
  min: {
    type: Number,
    required: true,
  },
  max: {
    type: Number,
    required: true,
  },
  disabled: {
    type: Boolean,
    default: false,
    required: false,
  },
});

const emit = defineEmits(["update:modelValue"]);

const customValue = (modelValue) => {
  if (modelValue === "") {
    return "";
  }
  const withoutSpaceValue = modelValue.toString().replace(/\s+/g, "");
  return parseInt(withoutSpaceValue);
};

const spacedValue = (modelValue) => {
  if (modelValue === "") {
    return "";
  }
  const spacedValue = parseInt(modelValue).toLocaleString();
  return spacedValue;
};

const customizedValue = ref(props.modelValue.toLocaleString());

const calcProgressLineSize = () => {
  const progressLineSize =
    ((props.modelValue - props.min) * 100) / (props.max - props.min);
  return progressLineSize;
};

const ProgressLineSize = computed(() => calcProgressLineSize());

const inputChange = (inputValue) => {
  let eventValue = parseInt(inputValue);
  if (isNaN(eventValue)) {
    eventValue = 0;
  }
  let value = 0;
  if (eventValue < props.min) {
    value = props.min;
  } else if (eventValue > props.max) {
    value = props.max;
  } else {
    value = eventValue;
  }

  emit("update:modelValue", value);
  customizedValue.value = parseInt(value);
  calcProgressLineSize();
};

const sliderChange = (value) => {
  emit("update:modelValue", parseInt(value));
  customizedValue.value = spacedValue(parseInt(value));
  calcProgressLineSize();
};
</script>

<style scoped lang="scss">
@use './Slider.scss';
</style>