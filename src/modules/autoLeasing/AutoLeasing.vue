<template>
  <div class="content">
    <h1 class="header">Рассчитайте стоимость автомобиля в лизинг</h1>
    <form class="form" @submit="$event.preventDefault()">
      <UISlider
        class="form__slider"
        label="Стоимость автомобиля"
        afterInput="₽"
        v-model="autoPrice"
        :min="1000000"
        :max="6000000"
        :disabled="disabled"
      >
      </UISlider>

      <UISlider
        class="form__slider"
        label="Первоначальный взнос"
        afterInput="%"
        v-model="downPaymentPercent"
        :min="10"
        :max="60"
        :disabled="disabled"
      >
        <template #input="{ unSpaceValue, value }">
          <div class="form__input-portion-wrapper">
            <div class="form__input-portion">
              {{ unSpaceValue(downPayment) }} ₽
            </div>
            <div class="form__input-mark">{{ value }}%</div>
          </div>
        </template>
      </UISlider>

      <UISlider
        class="form__slider"
        label="Срок лизинга"
        afterInput="мес."
        v-model="leaseTerm"
        :min="1"
        :max="60"
        :disabled="disabled"
      >
      </UISlider>
      <div class="form__text">
        <span> Сумма договора лизинга </span>
        {{ total.toLocaleString() }} ₽
      </div>
      <div class="form__text">
        <span> Ежемесячный платеж от </span>
        {{ monthPay.toLocaleString() }} ₽
      </div>
      <UIButton @click="send" class="form__button" :loading="loading">
        Оставить заявку
      </UIButton>
    </form>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import axios from "axios";
import UISlider from "../../components/slider/UISlider.vue";
import UIButton from "../../components/button/UIButton.vue";

const interestRate = 0.035;

const autoPrice = ref(3300000);
const leaseTerm = ref(60);
const downPaymentPercent = ref(13);
const loading = ref(false);
const disabled = ref(false);

const downPayment = computed(() => {
  return Math.round((downPaymentPercent.value / 100) * autoPrice.value);
});

const monthPay = computed(() => {
  return Math.round(
    (autoPrice.value - downPayment.value) *
      ((interestRate * Math.pow(1 + interestRate, leaseTerm.value)) /
        (Math.pow(1 + interestRate, leaseTerm.value) - 1))
  );
});

const total = computed(() => {
  return Math.round(downPayment.value + leaseTerm.value * monthPay.value);
});

const postRequest = async (info) => {
  try {
    await axios.post("https://eoj3r7f3r4ef6v4.m.pipedream.net", info);
  } catch (err) {
    return false;
  }
};

const send = () => {
  loading.value = true;
  disabled.value = true;

  const application = {
    autoPrice: autoPrice.value,
    downPaymentPercent: downPaymentPercent.value,
    leaseTerm: leaseTerm.value,
    monthPay: monthPay.value,
    total: total.value,
  };

  postRequest(application);

  setTimeout(() => {
    loading.value = false;
    disabled.value = false;
    console.log("send query");
  }, 5000);
};
</script>

<style scoped lang="scss">
@use "../../assets/styles/abstracts" as *;

.content {
  width: 100%;
}

.header {
  width: 100%;
  font-family: "Nekst", sans-serif;
  font-style: normal;
  font-weight: 900;
  font-size: 5.4rem;
  line-height: 90%;
  color: $black-color;
  margin-bottom: 3.2rem;

  @include media(">=huge") {
    width: 60%;
  }

  @include media(">=small", "<medium") {
    font-size: 3.4rem;
  }

  @include media("<small") {
    font-size: 3.4rem;
  }
}

.form {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  column-gap: 4.1rem;
  row-gap: 6.1rem;
  align-items: start;

  @include media(">=large", "<huge") {
    grid-template-columns: 1fr 1fr;
    row-gap: 4rem;
  }

  @include media(">=medium", "<large") {
    grid-template-columns: 1fr 1fr;
    row-gap: 2.2rem;
  }

  @include media(">=small", "<medium") {
    grid-template-columns: 1fr;
    row-gap: 1.6rem;
  }

  @include media("<small") {
    grid-template-columns: 1fr;
    row-gap: 1.6rem;
  }

  &__slider {
    @include media(">=medium", "<huge") {
      grid-column-start: 1;
      grid-column-end: 3;
    }
  }

  &__text {
    display: flex;
    flex-direction: column;
    font-family: "Nekst", sans-serif;
    font-style: normal;
    font-weight: 900;
    font-size: 5.4rem;
    color: $gray-color;

    @include media(">=small", "<medium") {
      font-size: 2.2rem;
    }

    @include media("<small") {
      font-size: 2.2rem;
    }

    & > span {
      font-family: "Gilroy", sans-serif;
      font-style: normal;
      font-weight: 400;
      font-size: 1.6rem;
      margin-bottom: 0.8rem;

      @include media(">=small", "<medium") {
        font-size: 1.4rem;
      }

      @include media("<small") {
        font-size: 1.4rem;
      }
    }
  }

  &__input-portion-wrapper {
    display: flex;
    width: 100%;
    align-items: center;
  }

  &__input-portion {
    width: 100%;
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

  &__input-mark {
    font-family: "Nekst", sans-serif;
    font-style: normal;
    font-weight: 900;
    font-size: 3rem;
    color: $gray-color;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 4.8rem;
    width: 6.7rem;
    position: absolute;
    left: calc(100% - 7.1rem);
    top: calc(100% - 0.5rem);
    translate: 0 -100%;
    font-size: 2rem;
    background: #ebebec;
    border-radius: 1.6rem;

    @include media(">=small", "<medium") {
      font-size: 2.2rem;
      top: 100%;
    }

    @include media("<small") {
      font-size: 2.2rem;
      top: 100%;
    }
  }
}
</style>