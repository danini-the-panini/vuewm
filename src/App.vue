<template>
  <div class="screen">
    <Desktop />
    <Window v-for="window in windows" :key="window.id" v-bind="window" />
  </div>
</template>

<script setup>
import { reactive } from 'vue';
import { v4 as uuid } from 'uuid';

import Desktop from '@/components/Desktop.vue';
import Window from '@/components/Window.vue';

const windows = reactive([{
  id: uuid(),
  x: 50,
  y: 50,
  width: 320,
  height: 240,
  title: 'Hello, World!',
}]);
</script>

<style scoped>
.screen {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
