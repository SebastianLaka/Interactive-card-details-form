<script setup>
const props = defineProps({
  cardData: {
    type: Object,
    required: true,
  },
})
</script>
<template>
  <section>
    <slot
      :cvc="props.cardData.cvc"
      :cardNumber="props.cardData.cardNumber"
      :cardOwner="props.cardData.cardOwner"
      :cardMonth="props.cardData.cardMonth"
      :cardYear="props.cardData.cardYear"
    ></slot>
  </section>
</template>
