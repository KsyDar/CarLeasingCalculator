<template>
  <div class="content">
    <h1 class="header">Рассчитайте стоимость автомобиля в лизинг</h1>
    <form class="form" @submit="$event.preventDefault()">
      <Slider
        label="Стоимость автомобиля"
        afterInput="₽"
        v-model="autoPrice"
        :min="1000000"
        :max="6000000"
        :disabled="disabled"
      >
      </Slider>

      <Slider
        label="Первоначальный взнос"
        afterInput="%"
        v-model="downPaymentPercent"
        :min="10"
        :max="60"
        :disabled="disabled"
      >
        <template #input="{ spacedValue, value }">
          <div class="form__input-portion-wrapper">
            <div class="form__input-portion">
              {{ spacedValue(downPayment) }} ₽
            </div>
            <div class="form__input-mark">{{ value }}%</div>
          </div>
        </template>
      </Slider>

      <Slider
        label="Срок лизинга"
        afterInput="мес."
        v-model="leaseTerm"
        :min="1"
        :max="60"
        :disabled="disabled"
      >
      </Slider>
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
import axios from 'axios'
import Slider from "../../../components/Slider/Slider.vue";
import UIButton from "../../../components/Button/UIButton.vue";

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

const postReguest = async (info) => {
  try {
    await axios.post("https://eoj3r7f3r4ef6v4.m.pipedream.net", info)
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

  postReguest(application);

  setTimeout(() => {
    loading.value = false;
    disabled.value = false;
    console.log("send query");
  }, 5000);
};
</script>

<style scoped lang="scss">
@use "AutoLeasing";

.content {
  width: 100%;
}

.submit-button {
  padding: 8px 16px 12px;
  width: 426px;
  height: 68px;
  background: #ff9514;
  border-radius: 40px;
  font-family: "Nekst";
  font-style: normal;
  font-weight: 600;
  font-size: 30px;
  line-height: 36px;
  color: #fff;
  border: none;

  &:hover {
    background: #111111;
    transition: background 0.5s;
    cursor: pointer;
  }

  &:active {
    background: #575757;
    transition: background 0.5s;
  }

  &:disabled {
    background: #ff951466;
    transition: background 0.5s;
  }
}
</style>