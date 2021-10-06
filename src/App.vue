<template>
  <div class="screen">
    <Desktop />
    <Window v-for="(window, index) in windows" :key="window.id" v-model="windows[index]" @focuswindow="focusWindow"/>
  </div>
</template>

<script setup>
import { reactive } from 'vue';
import { v4 as uuid } from 'uuid';

import Desktop from '@/components/Desktop.vue';
import Window from '@/components/Window.vue';

const windows = reactive([
  {
    id: uuid(),
    x: 50,
    y: 50,
    width: 320,
    height: 240,
    title: 'Hello, World!',
    z: 0,
  },
  {
    id: uuid(),
    x: 100,
    y: 100,
    width: 320,
    height: 240,
    title: 'Lorem Ipsum',
    z: 1,
  },
  {
    id: uuid(),
    x: 150,
    y: 150,
    width: 320,
    height: 240,
    title: 'Dolar sit amet',
    z: 2,
  },
]);

function focusWindow(window) {
  let oldZ = window.z;
  windows.forEach(w => {
    if (w.z >= oldZ) w.z -= 1;
  });
  window.z = windows.length - 1;
}
</script>

<style scoped>
.screen {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
