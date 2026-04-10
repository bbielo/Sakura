<script setup>
import SakuraEffect from "./components/SakuraEffect.vue";
</script>

<template>
  <SakuraEffect />

  <main class="content">
    <h1>🌸 Sakura Vue</h1>
    <p>벚꽃 애니메이션 (Vue version)</p>
  </main>
</template>

<style scoped>
.content {
  position: relative;
  z-index: 1;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
  color: white;
  text-align: center;
}
</style>