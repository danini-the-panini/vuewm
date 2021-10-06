<template>
  <div class="background"></div>
  <div class="icons">
    <div class="shortcut" @click="$emit('launchapp')">
      <img class="icon" src="../assets/icon.png" />
      <span class="label">New App</span>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';

defineEmits(['launchapp']);
</script>

<style scoped>
.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('../assets/wallpaper.png');
}

.icons {
  position: relative;
  padding: 16px;
}

.shortcut {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: fit-content;
}

.icon {
  width: 32px;
  height: 32px;
  margin-bottom: 4px;
}

.label {
  color: white;
  font-size: 12px;
  text-shadow: 1px 1px 1px rgb(0 0 0);
  user-select: none;
}
</style>
