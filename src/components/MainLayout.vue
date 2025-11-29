<script setup>
import HeaderArea from './HeaderArea.vue'
</script>
<template>
  <main class="main-area">
    <HeaderArea>
      <template #header>
        <div class="header-area"></div>
      </template>
    </HeaderArea>
  </main>
</template>
<style lang="scss" scoped>
@media (min-width: 320px) {
  .main-area {
    display: flex;
    flex-direction: column;
    .header-area {
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
    .header-area {
      background-image: url('../assets/images/bg-main-desktop.png');
      min-height: 100svh;
      grid-column: 1/6;
    }
  }
}
</style>
