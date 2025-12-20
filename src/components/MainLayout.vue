<script setup>
import { ref, computed } from 'vue'
import MainImageArea from './MainImageArea.vue'
import CardsArea from './CardsArea.vue'
import FormArea from './FormArea.vue'
import FormButton from './FormButton.vue'
import CardLogo from '../assets/icons/card-logo.svg'
const cardContentData = ref({
  cardNumber: '0000 0000 0000 0000',
  cardOwner: 'JANE APPLESSED',
  cardMonth: '00',
  cardYear: '00',
  cardLogo: CardLogo,
  cardAlt: 'Logo in card',
  cardCvc: '000',
})
const formModelData = ref({
  cardOwner: '',
  cardNumber: '',
  cardMonth: '',
  cardYear: '',
  cardCvc: '',
})
const formData = ref({
  name: {
    labelName: 'CARDHOLDER NAME',
    placeholder: 'e.g Jane Oldman',
    modelKey: 'cardOwner',
    maxLength: 30,
    error: '',
  },
  number: {
    labelName: 'CARD NUMBER',
    placeholder: 'e.g 1234 5678 9123 0000',
    modelKey: 'cardNumber',
    maxLength: 16,
    error: '',
  },
  month: {
    id: 3,
    labelName: 'EXP. DATE',
    placeholder: 'MM',
    modelKey: 'cardMonth',
    maxLength: 2,
    error: '',
  },
  year: {
    id: 4,
    labelName: '(MM/YY)',
    placeholder: 'YY',
    modelKey: 'cardYear',
    maxLength: 2,
    error: '',
  },
  cvc: {
    id: 5,
    labelName: 'CVC',
    placeholder: 'e.g. 123',
    modelKey: 'cardCvc',
    maxLength: 3,
    error: '',
  },
})
const readOwnerName = computed({
  get() {
    return formModelData.value.cardOwner
  },
  set(firstLetter) {
    formModelData.value.cardOwner = firstLetter.replace(/\b\w/g, (c) => c.toUpperCase());
  },
})

const validateForm = () => {
  const invalidChars = /[0-9!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]/
  if (readOwnerName.value.trim().length <= 1 || invalidChars.test(readOwnerName.value)) {
    formData.value.name.error = 'Name should contain only big or small letters and min. 2 letters.'
  } else {
    readOwnerName
    formData.value.name.error = ''
  }
}
</script>
<template>
  <main class="main-area">
    <MainImageArea>
      <template #header>
        <div class="background-area"></div>
      </template>
    </MainImageArea>
    <CardsArea :card-data="cardContentData">
      <template #default>
        <div class="card-back">
          <p class="card-back__cvc">{{ formModelData.cardCvc || cardContentData.cardCvc }}</p>
        </div>
        <div class="card-front">
          <img
            :src="cardContentData.cardLogo"
            :alt="cardContentData.cardAlt"
            class="card-front__card-image"
          />
          <div class="card-front-data">
            <p class="card-front-data__card-number">
              {{ formModelData.cardNumber || cardContentData.cardNumber }}
            </p>
            <div class="card-front-person-data">
              <p class="card-front-person-data__owner-name">
                {{ formModelData.cardOwner || cardContentData.cardOwner }}
              </p>
              <p class="card-front-person-data__owner-birth-date">
                {{ formModelData.cardMonth || cardContentData.cardMonth }}/{{
                  formModelData.cardYear || cardContentData.cardYear
                }}
              </p>
            </div>
          </div>
        </div>
      </template>
    </CardsArea>
    <form class="form-container" @submit.prevent="checkform">
      <FormArea class="form-field-1" :form-data="formData.name">
        <template #default>
          <label class="form-label-area"
            >{{ formData.name.labelName }}
            <input
              class="form-field"
              type="text"
              :placeholder="formData.name.placeholder"
              :maxlength="formData.name.maxLength"
              v-model="readOwnerName"
            />
            <p class="form-container__error-info">{{ formData.name.error }}</p>
          </label>
        </template>
      </FormArea>
      <FormArea class="form-field-2" :form-data="formData.number">
        <template #default>
          <label class="form-label-area"
            >{{ formData.number.labelName }}
            <input
              class="form-field"
              type="text"
              :placeholder="formData.number.placeholder"
              :maxlength="formData.number.maxLength"
              v-model="formModelData[formData.number.modelKey]"
            />
            <p class="form-container__error-info">{{ formData.number.error }}</p>
          </label>
        </template>
      </FormArea>
      <FormArea class="form-field-3" :form-data="formData.month">
        <template #default>
          <label class="form-label-area"
            >{{ formData.month.labelName }}
            <input
              class="form-field"
              type="text"
              :placeholder="formData.month.placeholder"
              :maxlength="formData.month.maxLength"
              v-model="formModelData[formData.month.modelKey]"
            />
            <p class="form-container__error-info">{{ formData.month.error }}</p>
          </label>
        </template>
      </FormArea>
      <FormArea class="form-field-4" :form-data="formData.year">
        <template #default>
          <label class="form-label-area"
            >{{ formData.year.labelName }}
            <input
              class="form-field"
              type="text"
              :placeholder="formData.year.placeholder"
              :maxlength="formData.year.maxLength"
              v-model="formModelData[formData.year.modelKey]"
            />
            <p class="form-container__error-info">{{ formData.year.error }}</p>
          </label>
        </template>
      </FormArea>
      <FormArea class="form-field-5" :form-data="formData.cvc">
        <template #default>
          <label class="form-label-area"
            >{{ formData.cvc.labelName }}
            <input
              class="form-field"
              type="text"
              :placeholder="formData.cvc.placeholder"
              :maxlength="formData.cvc.maxLength"
              v-model="formModelData[formData.cvc.modelKey]"
            />
            <p class="form-container__error-info">{{ formData.cvc.error }}</p>
          </label>
        </template>
      </FormArea>
      <FormButton @click="validateForm">Confirm</FormButton>
    </form>
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
      grid-template-columns: repeat(6, 1fr);
      grid-template-rows: repeat(3, 1fr);
      gap: 1.5em 0.05em;
      width: 65%;
      margin: 0 auto;
      .form-field-1,
      .form-field-2 {
        grid-column: 1/-1;
      }
      .form-field-2 {
        grid-row: 2/3;
      }
      .form-field-3,
      .form-field-4 {
        grid-column: 1/2;
        grid-row: 3/4;
        min-width: 25%;
      }
      .form-field-4 {
        grid-column: 2/3;
        min-width: 25%;
      }
      .form-field-5 {
        grid-column: 3/7;
        grid-row: 3/4;
        min-width: 50%;
      }
      .form-label-area {
        display: flex;
        flex-direction: column;
        justify-content: center;
        gap: 0.4em 0;
        font-size: 0.675rem;
        letter-spacing: 0.05em;
        .form-field {
          border-radius: 0.2em;
          border: 0.1em solid $gray-200;
          padding: 1em;
        }
      }
      &__error-info {
        font-size: 0.65rem;
        color: $red-400;
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
      grid-column: 7/-1;
      place-self: center;
    }
  }
}
@media (min-width: 1440px) {
  .main-area {
    .card-front {
      .card-front-data {
        &__card-number {
          word-spacing: 0.75vw;
        }
      }
    }
  }
}
</style>
