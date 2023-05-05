<template>
  <Resize :opt="{ min: 100 }">
    <ResizeItem>1</ResizeItem>
    <ResizeItem>2</ResizeItem>
    <ResizeItem>3</ResizeItem>
  </Resize>
</template>

<script setup lang="ts">
import Resize from './index.vue'
import ResizeItem from './ResizeItem.vue'
/**
 * @param {object}opt: 
 * @param {number}min: 单个容器的最小高度
 *  */
</script>

<style scoped></style>