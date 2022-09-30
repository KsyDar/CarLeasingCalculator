<template>
  <div class="main-content">
    <h1 class="main-title">Рассчитайте стоимость</h1>
    <h1 class="main-title">автомобиля в лизинг</h1>
    <div class="content__item">
      <div class="price-wrapper">
        <h3 class="content__subtitle">Стоимость автомобиля</h3>
        <div class="slider-wrapper">
          <div class="slider__text-wrapper">
            <p class="slider__text">3 300 000</p>
            <p class="slider__text">₽</p>
          </div>
          <input type="range" class="slider__input-range" />
          <div class="slider__input-range__progress-line"></div>
        </div>
      </div>
      <div class="down-payment-wrapper">
        <h3 class="content__subtitle">Первоначальный взнос</h3>
        <div class="slider-wrapper">
          <div class="slider__text-wrapper">
            <p class="slider__text">420 000 ₽</p>
            <div class="slider__text slider__text--percents">13%</div>
          </div>
          <input type="range" class="slider__input-range" />
          <div class="slider__input-range__progress-line"></div>
        </div>
      </div>
      <div class="lease-term-wrapper">
        <h3 class="content__subtitle">Срок лизинга</h3>
        <div class="slider-wrapper">
          <div class="slider__text-wrapper">
            <p class="slider__text">60</p>
            <p class="slider__text">мес.</p>
          </div>
          <input type="range" class="slider__input-range" />
          <div class="slider__input-range__progress-line"></div>
        </div>
      </div>
    </div>
    <div class="content__item">
      <div class="total__item">
        <h3 class="content__subtitle">Сумма договора лизинга</h3>
        <p class="total__item__sum">4 400 313 ₽</p>
      </div>
      <div class="total__item">
        <h3 class="content__subtitle">Ежемесячный платеж от</h3>
        <p class="total__item__sum">114 455 ₽</p>
      </div>
      <button class="submit-button">Оставить заявку</button>
    </div>
  </div>
</template>

<script setup>
</script>

<style lang="scss" scoped>
.main-content {
  margin: 21.3rem 4.7rem;
}
.main-title {
  font-family: "Nekst";
  font-style: normal;
  font-weight: 900;
  font-size: 54px;
  line-height: 90%;
  margin: 0;
}
.content__item {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-column-gap: 3.2rem;
  margin-top: 3.2rem;
  align-items: center;
}

.content__subtitle {
  font-family: "Gilroy";
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 20px;
  color: #575757;
}

.slider-wrapper {
  position: relative;
}

.slider__text-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #f3f3f4;
  border-radius: 16px;
  font-family: "Nekst";
  font-style: normal;
  font-weight: 900;
  font-size: 30px;
  line-height: 36px;
  color: #575757;
}

.slider__text {
  margin: 1.6rem 2.4rem;
}

.slider__text--percents {
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

.slider__input-range {
  appearance: none;
  background: #e1e1e1;
  width: 87%;
  height: 0.2rem;
  position: absolute;
  top: 64px;
  border-radius: 16px;
  margin-left: 2rem;
}

.slider__input-range::-webkit-slider-thumb {
  background: #ff9514;
  border: none;
  border-radius: 100%;
  cursor: pointer;
  width: 20px;
  height: 20px;
  -webkit-appearance: none;
}

.slider__input-range__progress-line {
  height: 0.2rem;
  width: 50%;
  background: #ff9514;
  position: absolute;
  top: 66px;
  border-radius: 16px;
  margin-left: 2rem;
}

.total__item__sum {
  font-family: "Nekst";
  font-style: normal;
  font-weight: 900;
  font-size: 54px;
  line-height: 90%;
  color: #575757;
  margin: 0;
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
}

.submit-button:hover {
  background: #111111;
  transition: background 0.5s;
  cursor: pointer;
}

.submit-button:active {
  background: #575757;
  transition: background 0.5s;
}

.submit-button:disabled {
  background: #ff951466;
  transition: background 0.5s;
}
</style>