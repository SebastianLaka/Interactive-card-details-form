<script setup>
import { ref } from 'vue'
import MainImageArea from './MainImageArea.vue'
import CardsArea from './CardsArea.vue'
import CardLogo from '../assets/icons/card-logo.svg'
const cvc = ref('000')
const frontCardData = ref({
  cardNumber: '0000 0000 0000 0000',
  cardOwner: 'JANE APPLESSED',
  cardMonth: '00',
  cardYear: '00',
  cardLogo: CardLogo,
  cardAlt: 'Logo in card',
})
</script>
<template>
  <main class="main-area">
    <MainImageArea>
      <template #header>
        <div class="background-area"></div>
      </template>
    </MainImageArea>
    <CardsArea :card-data="frontCardData">
      <template #default>
        <div class="card-back">
          <p class="card-back__cvc">{{ cvc }}</p>
        </div>
        <div class="card-front">
          <img
            :src="frontCardData.cardLogo"
            :alt="frontCardData.cardAlt"
            class="card-front__card-image"
          />
          <div class="card-front-data">
            <p class="card-front-data__card-number">{{ frontCardData.cardNumber }}</p>
            <div class="card-front-person-data">
              <p class="card-front-person-data__owner-name">{{ frontCardData.cardOwner }}</p>
              <p class="card-front-person-data__owner-birth-date">
                {{ frontCardData.cardMonth }}/{{ frontCardData.cardYear }}
              </p>
            </div>
          </div>
        </div>
      </template>
    </CardsArea>
    
  </main>
</template>
<style lang="scss" scoped>
@use '../assets/sass/colors' as *;
@media (min-width: 375px) {
  .main-area {
    display: flex;
    flex-direction: column;
    position: relative;
    .background-area {
      background-image: url('../assets/images/bg-main-mobile.png');
      min-height: 40svh;
      background-position: center center;
      background-size: cover;
    }
    .card-back,
    .card-front {
      background-position: center center;
      background-size: cover;
      background-repeat: no-repeat;
      border-radius: 0.8em;
      position: absolute;
      width: 75vw;
      height: 47vw; 
    }
    .card-back {
      background-image: url('../assets/images/bg-card-back.png');
      right: .75em;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      padding: 2em;
      top: 4.5em;
      &__cvc {
        color: $gray-200;
        padding-bottom: 0.25em;
      }
    }
    .card-front {
      background-image: url('../assets/images/bg-card-front.png');
      left: .75em;
      top: 164px;
      display: flex;
      justify-content: space-between;
      flex-direction: column;
      padding: 1.25em;
      transform: translateY(10vw);
      &__card-image {
        align-self: start;
        width: 4.25em;
        height: 2.5em;
      }
      .card-front-data {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        &__card-number {
          color: $gray-200;
          font-size: clamp(0.915rem, 2vw, 1.5rem);
          word-spacing: 2.4vw;
        }
        .card-front-person-data {
          display: flex;
          justify-content: space-between;
          align-items: center;
          &__owner-name,
          &__owner-birth-date {
            color: $gray-200;
            padding-top: .75em;
            font-size: .675rem;
          }
        }
      }
    }
  }
}
@media (min-width: 570px) {
  .main-area {
    .card-front,
    .card-back {
      width: 447px;
      height: 245px;
      top: auto;
      bottom: auto;
      transform: none;
    }
    .card-back {
      top: 50px;
      left: 55%;
      margin-left: -223.5px;
      right: auto;
    }
    .card-front {
      top: 188px;
      left: 37.5%;
      margin-left: -183.5px;
    }
  }
}

@media (min-width: 992px) {
  .main-area {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    .background-area {
      background-image: url('../assets/images/bg-main-desktop.png');
      min-height: 100svh;
      grid-column: 1/6;
    }
  }
}
</style>
