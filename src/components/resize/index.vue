<template>
  <div class="ass-resize flc" ref="containerRef">
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
import { onMounted, provide, ref } from 'vue';

const props = defineProps({
  opt: Object,
  default: () => ({ min: 0 })
})
provide('ass-resize-option', props)

const containerRef = ref();

onMounted(() => {
  const childElements = containerRef.value.children;
  const childElementCount = containerRef.value.childElementCount;
  const { height } = getComputedStyle(containerRef.value);
  Array.from(childElements).map((c) => {
    (<HTMLElement>c).style.height = `${parseFloat(height) / childElementCount}px`;
  })
})
</script>

<style scoped lang="less">
.ass-resize {
  height: 100%;
  overflow: hidden;

  :deep(.ass-resize-item) {
    &:nth-child(1)>.ass-resize-item-prefix {
      display: none;
    }
  }
}
</style>