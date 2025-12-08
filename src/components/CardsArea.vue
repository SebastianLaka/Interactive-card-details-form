<script setup>
const props = defineProps({
  cvc: String,
  cardData: Object,
});
</script>
<template>
  <section>
    <slot :cvc="props.cvc" :cardNumber="props.cardData.cardNumber" :cardOwner="props.cardData.cardOwner" :cardMonth="props.cardData.cardMonth" :cardYear="props.cardData.cardYear"></slot>
  </section>
</template>

