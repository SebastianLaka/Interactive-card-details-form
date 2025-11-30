<script setup>
import MainImageArea from './MainImageArea.vue'
import CardsArea from './CardsArea.vue'
</script>
<template>
  <main class="main-area">
    <MainImageArea>
      <template #header>
        <div class="background-area"></div>
      </template>
    </MainImageArea>
    <CardsArea>
      <template #default>
        <div class="card-back"></div>
        <div class="card-front"></div>
      </template>
    </CardsArea>
  </main>
</template>
<style lang="scss" scoped>
@media (min-width: 320px) {
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
      background-size: contain;
      background-repeat: no-repeat;
      border-radius: 0.8em;
      position: absolute;
      top: 5vw;
      bottom: 10vw;
    }
    .card-back {
      background-image: url('../assets/images/bg-card-back.png');
      width: 70vw;
      right: 4vw;
      left: auto;
    }
    .card-front {
      background-image: url('../assets/images/bg-card-front.png');
      width: 70vw;
      left: 4vw;
      right: auto;
      top: 7.7vw;
      transform: translateY(20.5vw);
    }
  }
}
@media (min-width: 585px){
  .main-area{
    .card-front, .card-back{
      width: 447px;
      height: 245px;
      top: auto;
      bottom: auto;
      transform: none;
    }
     .card-back{
      top: 50px; 
      left: 60%;
      margin-left: -223.5px; 
      right: auto; 
    }
    .card-front{
    
      top: 188px; 
      
      /* Poziom: +40px w prawo od karty tylnej (-223.5px + 40px) */
      left: 45%;
      margin-left: -183.5px; /* -223.5px + 40px */
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
