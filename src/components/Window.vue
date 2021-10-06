<template>
  <div class="window" :style="windowStyle" v-if="modelValue" @mousedown="$emit('focuswindow', modelValue)">
    <div class="handle top" @mousedown="resizeTop"></div>
    <div class="handle right" @mousedown="resizeRight"></div>
    <div class="handle bottom" @mousedown="resizeBottom"></div>
    <div class="handle left" @mousedown="resizeLeft"></div>
    <div class="handle top-left" @mousedown="resizeTopLeft"></div>
    <div class="handle top-right" @mousedown="resizeTopRight"></div>
    <div class="handle bottom-left" @mousedown="resizeBottomLeft"></div>
    <div class="handle bottom-right" @mousedown="resizeBottomRight"></div>
    <div class="inner" @mousedown="moveWindow">
      <div class="blur" :style="blurStyle"></div>
      <div class="border"></div>
      <div class="content"></div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, computed } from 'vue';

const props = defineProps({
  modelValue: Object,
});

const emit = defineEmits(['update:modelValue', 'focuswindow']);

const windowStyle = computed(() => ({
  left: `${props.modelValue.x}px`,
  top: `${props.modelValue.y}px`,
  width: `${props.modelValue.width}px`,
  height: `${props.modelValue.height}px`,
  zIndex: props.modelValue.z
}));

const blurStyle = computed(() => ({
  backgroundPosition: `${-props.modelValue.x + 32}px ${-props.modelValue.y + 32}px`,
}));

function doWhileDragging(cb) {
  return event => {
    let x = event.pageX;
    let y = event.pageY;

    const drag = (e) => {
      let dx = e.pageX - x;
      let dy = e.pageY - y;

      x = e.pageX;
      y = e.pageY;

      cb(dx, dy);
    };

    const stopDrag = () => {
      window.removeEventListener('mousemove', drag);
      window.removeEventListener('mouseup', stopDrag);
    };

    window.addEventListener('mousemove', drag);
    window.addEventListener('mouseup', stopDrag);
  };
}

const moveWindow = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    x: props.modelValue.x + dx,
    y: props.modelValue.y + dy,
  });
});

const resizeTop = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    y: props.modelValue.y + dy,
    height: props.modelValue.height - dy,
  });
});

const resizeBottom = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    height: props.modelValue.height + dy,
  });
});

const resizeLeft = doWhileDragging((dx) => {
  emit('update:modelValue', {
    ...props.modelValue,
    x: props.modelValue.x + dx,
    width: props.modelValue.width - dx,
  });
});

const resizeRight = doWhileDragging((dx) => {
  emit('update:modelValue', {
    ...props.modelValue,
    width: props.modelValue.width + dx,
  });
});

const resizeTopLeft = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    x: props.modelValue.x + dx,
    y: props.modelValue.y + dy,
    width: props.modelValue.width - dx,
    height: props.modelValue.height - dy,
  });
});

const resizeTopRight = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    y: props.modelValue.y + dy,
    width: props.modelValue.width + dx,
    height: props.modelValue.height - dy,
  });
});

const resizeBottomLeft = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    x: props.modelValue.x + dx,
    width: props.modelValue.width - dx,
    height: props.modelValue.height + dy,
  });
});

const resizeBottomRight = doWhileDragging((dx, dy) => {
  emit('update:modelValue', {
    ...props.modelValue,
    width: props.modelValue.width + dx,
    height: props.modelValue.height + dy,
  });
});
</script>

<style scoped>
.window {
  position: absolute;
}

.inner {
  border-radius: 8px;
  overflow: hidden;
  position: relative;
  box-shadow: 0px 16px 16px rgb(0 0 0 / 60%);
  width: 100%;
  height: 100%;
}

.blur {
  position: absolute;
  left: -32px;
  top: -32px;
  right: -32px;
  bottom: -32px;
  background-image: url('../assets/wallpaper.png');
  filter: blur(16px);
  pointer-events: none;
}

.border {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: 1px solid rgb(0 0 0 / 20%);
  border-radius: inherit;
}

.content {
  background: rgb(255 255 255 / 20%);
  width: 100%;
  height: 100%;
  position: relative;
}

.handle {
  position: absolute;
}

.top {
  top: -8px;
  height: 8px;
  left: 0;
  width: 100%;
  cursor: ns-resize;
}

.right {
  top: 0;
  left: 100%;
  width: 8px;
  height: 100%;
  cursor: ew-resize;
}

.bottom {
  top: 100%;
  left: 0;
  width: 100%;
  height: 8px;
  cursor: ns-resize;
}

.left {
  left: -8px;
  top: 0;
  width: 8px;
  height: 100%;
  cursor: ew-resize;
}

.top-left {
  left: -8px;
  top: -8px;
  width: 16px;
  height: 16px;
  cursor: nwse-resize;
}

.top-right {
  left: calc(100% - 8px);
  top: -8px;
  width: 16px;
  height: 16px;
  cursor: nesw-resize;
}

.bottom-left {
  top: calc(100% - 8px);
  left: -8px;
  width: 16px;
  height: 16px;
  cursor: nesw-resize;
}

.bottom-right {
  top: calc(100% - 8px);
  left: calc(100% - 8px);
  width: 16px;
  height: 16px;
  cursor: nwse-resize;
}
</style>
