<template>
  <div class="window" :style="windowStyle" @mousedown="startDrag" v-if="modelValue">
    <div class="blur" :style="blurStyle"></div>
    <div class="content"></div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, computed } from 'vue';

const props = defineProps({
  modelValue: Object,
});

const emit = defineEmits(['update:modelValue']);

const windowStyle = computed(() => ({
  left: `${props.modelValue.x}px`,
  top: `${props.modelValue.y}px`,
  width: `${props.modelValue.width}px`,
  height: `${props.modelValue.height}px`,
}));

const blurStyle = computed(() => ({
  backgroundPosition: `${-props.modelValue.x + 32}px ${-props.modelValue.y + 32}px`,
}));

let mouse = { x: null, y: null };

function drag(event) {
  let offsetX = event.pageX - mouse.x;
  let offsetY = event.pageY - mouse.y;

  mouse.x = event.pageX;
  mouse.y = event.pageY;

  emit('update:modelValue', {
    ...props.modelValue,
    x: props.modelValue.x + offsetX,
    y: props.modelValue.y + offsetY,
  });
}

function startDrag(event) {
  mouse.x = event.pageX;
  mouse.y = event.pageY;
  
  const stopDrag = () => {
    window.removeEventListener('mousemove', drag);
    window.removeEventListener('mouseup', stopDrag);
  };

  window.addEventListener('mousemove', drag);
  window.addEventListener('mouseup', stopDrag);
}
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
