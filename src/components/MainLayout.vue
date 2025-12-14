<script setup>
import { ref } from 'vue'
import MainImageArea from './MainImageArea.vue'
import CardsArea from './CardsArea.vue'
import FormArea from './FormArea.vue'
import FormButton from './FormButton.vue'
import CardLogo from '../assets/icons/card-logo.svg'
const cardData = ref({
  cardNumber: '0000 0000 0000 0000',
  cardOwner: 'JANE APPLESSED',
  cardMonth: '00',
  cardYear: '00',
  cardLogo: CardLogo,
  cardAlt: 'Logo in card',
  cardCvc: '000',
})
const formsData = ref([
  {
    id: 1,
    labelName: 'CARDHOLDER NAME',
    placeholder: 'e.g Jane Oldman',
  },
  {
    id: 2,
    labelName: 'CARD NUMBER',
    placeholder: 'e.g 1234 5678 9123 0000',
  },
  {
    id: 3,
    labelName: 'EXP. DATE',
    placeholder: 'MM',
  },
  {
    id: 4,
    labelName: '(MM/YY)',
    placeholder: 'YY',
  },
  {
    id: 5,
    labelName: 'CVC',
    placeholder: 'e.g. 123',
  },
])
</script>
<template>
  <main class="main-area">
    <MainImageArea>
      <template #header>
        <div class="background-area"></div>
      </template>
    </MainImageArea>
    <CardsArea :card-data="cardData">
      <template #default>
        <div class="card-back">
          <p class="card-back__cvc">{{ cardData.cardCvc }}</p>
        </div>
        <div class="card-front">
          <img :src="cardData.cardLogo" :alt="cardData.cardAlt" class="card-front__card-image" />
          <div class="card-front-data">
            <p class="card-front-data__card-number">{{ cardData.cardNumber }}</p>
            <div class="card-front-person-data">
              <p class="card-front-person-data__owner-name">{{ cardData.cardOwner }}</p>
              <p class="card-front-person-data__owner-birth-date">
                {{ cardData.cardMonth }}/{{ cardData.cardYear }}
              </p>
            </div>
          </div>
        </div>
      </template>
    </CardsArea>
    <div class="form-container">
      <FormArea
        v-for="field in formsData"
        :key="field.id"
        :form-data="field"
        :class="`form-field-${field.id}`"
      >
        <template #default>
          <label :for="field.id" class="form-label-area"
            >{{ field.labelName }}
            <input
              :id="field.id"
              type="text"
              class="form-field"
              :placeholder="field.placeholder"
              :name="field.id"
            />
          </label>
        </template>
      </FormArea>
      <FormButton>Confirm</FormButton>
    </div>
  </main>
</template>
<style lang="scss" scoped>
@use '../assets/sass/colors' as *;
@media (min-width: 375px) {
  .main-area {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100svh;
    position: relative;
    padding-bottom: 1em;
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
      right: 0.75em;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      padding: 2em 1.25em;
      top: 4.5em;
      &__cvc {
        color: $gray-200;
        padding-bottom: 0.25em;
      }
    }
    .card-front {
      background-image: url('../assets/images/bg-card-front.png');
      left: 0.75em;
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
          font-size: clamp(1.2rem, 5.3vw, 2rem);
          text-align: center;

          word-spacing: 1.5vw;
        }
        .card-front-person-data {
          display: flex;
          justify-content: space-between;
          align-items: center;
          &__owner-name,
          &__owner-birth-date {
            color: $gray-200;
            padding-top: 0.75em;
            font-size: 0.675rem;
          }
        }
      }
    }
    .form-container {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: repeat(3, 1fr);
      gap: 1.5em 0.5em;
      padding: 1em 1em 0;
      .form-field-1,
      .form-field-2 {
        grid-column: 1/4;
      }
      .form-field-2 {
        grid-row: 2/3;
      }
      .form-field-3 {
        grid-column: 1/1;
        grid-row: 3/4;
        min-width: 25%;
      }
      .form-field-4 {
        grid-column: 2/3;
        grid-row: 3/4;
        min-width: 25%;
      }
      .form-field-5 {
        grid-column: 3/4;
        grid-row: 3/4;
        min-width: 50%;
      }
      .form-label-area {
        display: flex;
        flex-direction: column;
        justify-content: center;
        gap: 0.4em 0;
        font-size: 0.875rem;
        letter-spacing: 0.1em;
        .form-field {
          border-radius: 0.2em;
          border: 0.1em solid $gray-200;
          padding: 1em;
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
      padding: 3em;
    }
    .card-front {
      top: 188px;
      left: 37.5%;
      margin-left: -183.5px;
      .card-front-data {
        &__card-number {
          font-size: clamp(1.5rem, 6vw, 2rem);
          word-spacing: 1.75vw;
        }
      }
    }
    .form-container {
      margin: 0 auto;
    }
  }
}
@media (min-width: 992px) {
  .main-area {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: repeat(3, 1fr);
    .background-area {
      background-image: url('../assets/images/bg-main-desktop.png');
      min-height: 100svh;
      grid-column: 1/5;
    }
    .card-front {
      top: 15%;
      left: 25%;
      .card-front-data {
        &__card-number {
          word-spacing: 1vw;
        }
      }
    }
    .card-back {
      top: 50%;
      left: 30%;
      padding: 3em;
    }
    .form-container {
      grid-column: 8/12;
      place-self: center;
    }
  }
}
@media (min-width: 1440px) {
  .main-area {
    .form-container {
      grid-column: 8/11;
    }
  }
}
</style>
