<script setup>
import { ref, computed, reactive } from 'vue'
import MainImageArea from './MainImageArea.vue'
import CardsArea from './CardsArea.vue'
import FormArea from './FormArea.vue'
import SuccessNotification from './SuccessNotification.vue'
import FormButton from './FormButton.vue'
import CardLogo from '../assets/icons/card-logo.svg'
import CompleteIcon from '../assets/icons/icon-complete.svg'
const isVisible = ref(false)
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
    maxLength: 19,
    error: '',
  },
  month: {
    id: 3,
    labelName: 'EXP. DATE (MM)',
    placeholder: 'MM',
    modelKey: 'cardMonth',
    maxLength: 2,
    error: '',
  },
  year: {
    id: 4,
    labelName: '(YY)',
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
    formModelData.value.cardOwner = firstLetter.replace(/\b\w/g, (c) => c.toUpperCase())
  },
})
const readCardNumer = computed({
  get() {
    return formModelData.value.cardNumber
  },
  set(number) {
    const clearValue = number.replace(/\D/g, '')
    if (clearValue.length > 16) {
      clearValue = clearValue.substring(0, 16)
    }
    const format = clearValue.match(/.{1,4}/g)?.join(' ') || ''
    formModelData.value.cardNumber = format
  },
})
const readCardMonth = computed({
  get() {
    return formModelData.value.cardMonth
  },
  set(month) {
    let assignMonth = month.replace(/\D/g, '')
    if (assignMonth.length > 2) {
      assignMonth = assignMonth.substring(0, 2)
    }
    formModelData.value.cardMonth = assignMonth
  },
})
const readCardYear = computed({
  get() {
    return formModelData.value.cardYear
  },
  set(year) {
    const assignYear = year.replace(/[^0-9]/g, '')
    formModelData.value.cardYear = assignYear
  },
})
const readCardCvc = computed({
  get() {
    return formModelData.value.cardCvc
  },
  set(cvc) {
    const assignCvc = cvc.replace(/[^0-9]/g, '')
    formModelData.value.cardCvc = assignCvc
  },
})
const validateName = () => {
  const invalidChars = /[0-9!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]/
  if (readOwnerName.value.trim().length <= 1 || invalidChars.test(readOwnerName.value)) {
    formData.value.name.error = 'Name should contain only letters and min. 2 letters.'
  } else {
    formData.value.name.error = ''
  }
}
const validateCardNumber = () => {
  const regexp = /^\d+$/
  if (regexp.test(readCardNumer.value)) {
    formData.value.number.error = 'Wrong format, numbers only.'
  } else if (readCardNumer.value.length < 19) {
    formData.value.number.error = 'Number should contain only 16 numbers'
  } else {
    formData.value.number.error = ''
  }
}
const validateMonth = () => {
  if (readCardMonth.value.length === 1) {
    readCardMonth.value = readCardMonth.value.padStart(2, '0')
    formModelData.value.cardMonth = readCardMonth.value
  }
  if (readCardMonth.value === '' || readCardMonth.value === '00') {
    formData.value.month.error = "Can't be blank"
  } else if (readCardMonth.value < 1 || readCardMonth.value > 12) {
    formData.value.month.error = 'Should be 01-12'
  } else {
    formData.value.month.error = ''
  }
}
const validateYear = () => {
  if (readCardYear.value === '') {
    formData.value.year.error = "Can't be blank"
  } else {
    formData.value.year.error = ''
  }
}
const validateCvc = () => {
  if (readCardCvc.value === '') {
    formData.value.cvc.error = "Can't be blank"
  } else if (readCardCvc.value.length < 3) {
    formData.value.cvc.error = 'CVC should contain 3 numbers.'
  } else {
    formData.value.cvc.error = ''
  }
}

const validateForm = () => {
  validateName()
  validateCardNumber()
  validateMonth()
  validateYear()
  validateCvc()
  const getErrorValue = Object.values(formData.value).find((value) => value.error !== '')
  if (!getErrorValue) {
    return (isVisible.value = true)
  }
}
const handleReset = () => {
  formModelData.value = {
    cardOwner: '',
    cardNumber: '',
    cardMonth: '',
    cardYear: '',
    cardCvc: '',
  }
  isVisible.value = false
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
    <form v-if="!isVisible" class="form-container" @submit.prevent="checkform">
      <FormArea class="form-field-1" :form-data="formData.name">
        <template #default>
          <label class="form-label-area"
            >{{ formData.name.labelName }}
            <input
              class="form-field"
              type="text"
              :placeholder="formData.name.placeholder"
              :maxlength="formData.name.maxLength"
              :class="{ 'error-color': formData.name.error, 'marked-field-color': readOwnerName.length > 0 && !formData.name.error}"
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
              :class="{ 'error-color': formData.number.error, 'marked-field-color': readCardNumer.length === 19 && !formData.name.error }"
              v-model="readCardNumer"
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
              :class="{ 'error-color': formData.month.error, 'marked-field-color': readCardMonth.length >= 1 && !formData.name.error  }"
              v-model="readCardMonth"
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
              :class="{ 'error-color': formData.year.error, 'marked-field-color': readCardYear.length >= 1 && !formData.name.error }"
              v-model="readCardYear"
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
              :class="{ 'error-color': formData.cvc.error, 'marked-field-color': readCardCvc.length === 3 && !formData.name.error }"
              v-model="readCardCvc"
            />
            <p class="form-container__error-info">{{ formData.cvc.error }}</p>
          </label>
        </template>
      </FormArea>
      <FormButton class="form-container__button" @click="validateForm">Confirm</FormButton>
    </form>
    <SuccessNotification v-else>
      <template #default>
        <div class="success-notification-container">
          <h1 class="success-notification-container__header">THANK YOU</h1>
          <p class="success-notification-container__description">We've added your card details</p>
          <FormButton @click="handleReset" class="success-notification-container__button"
            >Continue</FormButton
          >
        </div>
      </template>
    </SuccessNotification>
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
      gap: 1.5em 0.5em;
      width: 80%;
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
        grid-column: 1/3;
        grid-row: 3/4;
        min-width: 25%;
      }
      .form-field-4 {
        grid-column: 3/5;
        min-width: 25%;
      }
      .form-field-5 {
        grid-column: 5/7;
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
          border-radius: 0.8em;
          border: 0.1em solid $gray-200;
          padding: 1em;
        }
        .error-color {
          border-color: $red-400;
        }
        .marked-field-color {
          border-color: $purple-950;
        }
      }
      &__error-info {
        font-size: 0.65rem;
        color: $red-400;
      }
      &__button {
        grid-column: 1/ -1;
      }
    }

    .success-notification-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 2em 0;
      width: 75%;
      margin: 0 auto;
      &__header {
        font-size: 2rem;
        letter-spacing: 0.1em;
        color: $purple-950;
      }
      &__description {
        color: $gray-400;
      }
      &__button {
        width: 100%;
      }
    }
    .fade-enter-active,
    .fade-leave-active {
      transition: opacity 0.3s ease;
    }

    .fade-enter-from,
    .fade-leave-to {
      opacity: 0;
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
      .form-field-3,
      .form-field-4 {
        grid-column: 1/3;
      }
      .form-field-4 {
        grid-column: 3/5;
      }
      .form-field-5 {
        grid-column: 5/7;
      }
    }
  }
}
@media (min-width: 768px) {
  .main-area {
    .form-container {
      gap: 1.5em 1em;
      .form-field-3,
      .form-field-4 {
        grid-column: 1/2;
      }
      .form-field-4 {
        grid-column: 2/3;
      }
      .form-field-5 {
        grid-column: 3/7;
      }
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
      grid-template-columns: repeat(12, 1fr);
      grid-template-rows: repeat(3, 1fr);
      grid-column: 7/-1;
      place-self: center;
      .form-field-3,
      .form-field-4 {
        grid-column: 1/4;
      }
      .form-field-4 {
        grid-column: 4/7;
      }
      .form-field-5 {
        grid-column: 7/ -1;
      }
    }
    .success-notification-container {
      grid-column: 7/-1;
      place-self: center;
    }
  }
}
@media (min-width: 1200px) {
  .main-area {
    .form-container {
      .form-field-3,
      .form-field-4 {
        grid-column: 1/4;
      }
      .form-field-4 {
        grid-column: 4/7;
      }
      .form-field-5 {
        grid-column: 7/ -1;
      }
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
    .form-container {
      .form-field-1,
      .form-field-2 {
        grid-column: 1/ 10;
      }
      .form-field-3,
      .form-field-4 {
        grid-column: 1/3;
      }
      .form-field-4 {
        grid-column: 3/5;
      }
      .form-field-5 {
        grid-column: 5/ 10;
      }
      &__button {
        grid-column: 1/ 10;
      }
    }
  }
}
</style>
