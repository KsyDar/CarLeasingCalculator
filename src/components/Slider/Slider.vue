<template>
  <div class="slider">
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
@use "../../assets/styles/abstracts/variables" as *;
.slider {
  position: relative;

  &__label {
    margin-bottom: 2.4rem;
    font-family: "Gilroy", sans-serif;
    font-style: normal;
    font-weight: 400;
    font-size: 1.6rem;
    transition: opacity 0.2s ease-in-out;
  }

  &__text-wrapper {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    padding: 1rem 2.4rem;
    background: $light-gray-bg-color;
    border-radius: 1.6rem;
    border: 2px solid $light-gray-bg-color;
    transition: background 0.2s ease-in-out, border 0.2s ease-in-out;

    &:focus-within {
      border: 2px solid $light-gray-bg-color;
      background: $white-color;
    }
  }

  &__input-text {
    width: 100%;
    background: transparent;
    border: none;
    outline: none;
    color: $gray-color;

    font-family: "Nekst-Black", sans-serif;
    font-style: normal;
    font-weight: 900;
    font-size: 3rem;
  }

  &__text {
    margin: 1.6rem 2.4rem;

    &--percents {
      width: 69px;
      height: 54px;
      background: #ebebec;
      border-radius: 16px;
      position: absolute;
      right: -16px;
      font-size: 20px;
      padding: 15px 17px;
      line-height: 120%;
    }
  }

  &__input-mark {
    font-family: "Nekst-Black", sans-serif;
    font-style: normal;
    font-weight: 900;
    font-size: 3rem;
    color: $gray-color;
  }

  &__range-wrapper {
    position: absolute;
    width: 100%;
    padding: 0 2.4rem;
    top: 100%;
    translate: 0 -85%;
  }

  &__input-range {
    -webkit-appearance: none;
    width: 100%;
    height: 0.2rem;
    outline: none;
    border-radius: 0.5rem;
    margin: 0;
    background-color: $slider-bg-bar-color;
    background-image: linear-gradient($orange-bg-color, $orange-bg-color);
    background-size: 50%;
    background-repeat: no-repeat;

    &::-webkit-slider-thumb {
      height: 2rem;
      width: 2rem;
      -webkit-appearance: none;
      border-radius: 50%;
      background: $orange-bg-color;
      box-shadow: 0 0 2px 0 #555;
      transition: background 0.3s ease-in-out, scale 0.3s ease-in-out;

      &:hover {
        width: 24px;
        height: 24px;
        transition: width 3s height 3s;
      }
    }

    &::-moz-range-thumb {
      height: 2rem;
      width: 2rem;
      -webkit-appearance: none;
      border-radius: 50%;
      background: $orange-bg-color;
      box-shadow: 0 0 2px 0 #555;
      transition: background 0.3s ease-in-out, scale 0.3s ease-in-out;

      &:hover {
        width: 24px;
        height: 24px;
        transition: width 3s height 3s;
      }
    }

    &::-ms-thumb {
      height: 2rem;
      width: 2rem;
      -webkit-appearance: none;
      border-radius: 50%;
      background: $orange-bg-color;
      box-shadow: 0 0 2px 0 #555;
      transition: background 0.3s ease-in-out, scale 0.3s ease-in-out;

      &:hover {
        width: 24px;
        height: 24px;
        transition: width 3s height 3s;
      }
    }

    &::-webkit-slider-runnable-track {
      -webkit-appearance: none;
      box-shadow: none;
      border: none;
      outline: none;
      background: transparent;
    }

    &::-moz-range-track {
      -webkit-appearance: none;
      box-shadow: none;
      border: none;
      outline: none;
      background: transparent;
    }

    &::-ms-track {
      -webkit-appearance: none;
      box-shadow: none;
      border: none;
      outline: none;
      background: transparent;
    }
  }
}
</style>