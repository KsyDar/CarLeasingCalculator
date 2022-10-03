<template>
  <div class="slider" :class="{ 'slider-disabled': disabled }">
    <div class="slider__label">
      {{ label }}
    </div>
    <div class="slider__text-wrapper" tabindex="0">
      <slot
        name="input"
        :value="modelValue"
        :converted-value="convertedValue"
        :unSpaceValue="unSpaceValue"
      >
        <input
          type="text"
          v-model="convertedValue"
          @change="inputChange($event.target.value)"
          @focus="convertedValue = convertValue(convertedValue)"
          @blur="convertedValue = unSpaceValue(convertedValue)"
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
        :style="{ backgroundSize: progressLineSize + '%' }"
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

const convertValue = (modelValue) => {
  if (modelValue === "") {
    return "";
  }
  const withoutSpaceValue = modelValue.toString().replace(/\s+/g, "");
  return parseInt(withoutSpaceValue);
};

const unSpaceValue = (modelValue) => {
  if (modelValue === "") {
    return "";
  }
  const spacedValue = parseInt(modelValue).toLocaleString();
  return spacedValue;
};

const convertedValue = ref(props.modelValue.toLocaleString());

const calcProgressLineSize = () => {
  const progressLineSize =
    ((props.modelValue - props.min) * 100) / (props.max - props.min);
  return progressLineSize;
};

const progressLineSize = computed(() => calcProgressLineSize());

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
  convertedValue.value = value;
  calcProgressLineSize();
};

const sliderChange = (value) => {
  const valueInt = parseInt(value);
  emit("update:modelValue", valueInt);
  convertedValue.value = unSpaceValue(valueInt);
  calcProgressLineSize();
};
</script>

<style scoped lang="scss">
@use './UISlider.scss';
</style>