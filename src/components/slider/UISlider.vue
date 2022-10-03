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
@use "../../assets/styles/abstracts" as *;

.slider {
    position: relative;

    &__label {
        margin-bottom: 2.4rem;
        font-family: "Gilroy", sans-serif;
        font-style: normal;
        font-weight: 400;
        font-size: 1.6rem;
        transition: opacity 0.2s ease-in-out;

        @include media(">=small", "<medium") {
            font-size: 1.4rem;
            margin-bottom: 0.8rem;
        }

        @include media("<small") {
            font-size: 1.4rem;
            margin-bottom: 0.8rem;
        }
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
        outline: none;

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

        font-family: "Nekst", sans-serif;
        font-style: normal;
        font-weight: 900;
        font-size: 3rem;

        @include media(">=small", "<medium") {
            font-size: 2.2rem;
        }

        @include media("<small") {
            font-size: 2.2rem;
        }
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

            @include media(">=small", "<medium") {
                font-size: 2.2rem;
            }

            @include media("<small") {
                font-size: 2.2rem;
            }
        }
    }

    &__input-mark {
        font-family: "Nekst", sans-serif;
        font-style: normal;
        font-weight: 900;
        font-size: 3rem;
        color: $gray-color;

        @include media(">=small", "<medium") {
            font-size: 2.2rem;
        }

        @include media("<small") {
            font-size: 2.2rem;
        }
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
                cursor: pointer;
                scale: 122%;
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
                cursor: pointer;
                scale: 122%;
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
                scale: 122%;
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

.slider-disabled {
    opacity: 0.5;
}
</style>