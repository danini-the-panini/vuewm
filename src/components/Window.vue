<template>
  <div class="window" :style="windowStyle">
    <div class="blur"></div>
    <div class="content"></div>
  </div>
</template>

<script setup>
import { defineProps, computed } from 'vue';

const props = defineProps({
  x: Number,
  y: Number,
  width: Number,
  height: Number,
});

const windowStyle = computed(() => ({
  left: `${props.x}px`,
  top: `${props.y}px`,
  width: `${props.width}px`,
  height: `${props.height}px`,
}));
</script>

<style scoped>
.window {
  position: absolute;
  border-radius: 8px;
  box-shadow: 0px 16px 16px rgb(0 0 0 / 60%);
  overflow: hidden;
}

.blur {
  position: absolute;
  left: -32px;
  top: -32px;
  right: -32px;
  bottom: -32px;
  background-image: url('../assets/wallpaper.png');
  filter: blur(16px);
}

.content {
  background: rgb(255 255 255 / 20%);
  width: 100%;
  height: 100%;
  position: relative;
}
</style>
