<template>
  <div class="screen">
    <Desktop @launchapp="launchApp"/>
    <Window
      v-for="(window, index) in windows"
      :key="window.id"
      v-model="windows[index]"
      @windowfocus="focusWindow"
      @windowclose="closeWindow"
    />
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
    y: 100,
    width: 320,
    height: 240,
    title: 'Hello, World!',
    z: 0,
  },
]);

function focusWindow(window) {
  let oldZ = window.z;
  windows.forEach(w => {
    if (w.z >= oldZ) w.z -= 1;
  });
  window.z = windows.length - 1;
}

function closeWindow(window) {
  windows.splice(windows.indexOf(window), 1);
  windows.forEach(w => {
    if (w.z > window.z) w.z -= 1;
  });
}

function launchApp() {
  let newWindow = {
    id: uuid(),
    x: 150,
    y: 150,
    width: 320,
    height: 240,
    title: 'New Window',
    z: windows.length,
  };

  windows.push(newWindow);
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
