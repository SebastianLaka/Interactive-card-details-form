<script setup>
import MainImageArea from './MainImageArea.vue'
</script>
<template>
  <main class="main-area">
    <MainImageArea>
      <template #header>
        <div class="background-area"></div>
      </template>
    </MainImageArea>
  </main>
</template>
<style lang="scss" scoped>
@media (min-width: 320px) {
  .main-area {
    display: flex;
    flex-direction: column;
    .background-area {
      background-image: url('../assets/images/bg-main-mobile.png');
      min-height: 40svh;
      background-position: center center;
      background-size: cover;
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
