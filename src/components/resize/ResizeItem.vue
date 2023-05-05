<template>
  <div class="ass-resize-item">
    <div class="ass-resize-item-prefix" @mousedown="$event => mousedownHandle($event)"></div>
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
import * as _ from 'lodash';
import { inject } from 'vue';

const {opt: {min}} = inject('ass-resize-option') as any;
const computePx = (a: string | number, b: string | number, min = 0, max: number) => {
  const h = parseFloat(a.toString());
  const offsetY = parseFloat(b.toString());
  let value = Math.min(max - min, h + offsetY);
  value = Math.max(min, value);
  return value;
}

const mousedownHandle = (ev: MouseEvent) => {
  const event = ev || window.Event;
  const dragger = ev.target as HTMLElement;
  const first = dragger.parentElement?.previousElementSibling as HTMLElement;
  const second = dragger.parentElement;
  if (!first || !second) return;
  const { height: h_f } = getComputedStyle(first);
  const { height: h_s } = getComputedStyle(second);
  const max = parseFloat(h_f) + parseFloat(h_s);
  const { clientY } = event;
  const resizeMove = _.throttle((ev: MouseEvent) => {
    const { clientY: y } = ev;
    const diffy = y - clientY;
    first.style.height = `${computePx(h_f, diffy, min, max)}px`
    second.style.height = `${computePx(h_s, -diffy, min, max)}px`
  }, 50)
  window.addEventListener('mousemove', resizeMove)
  window.addEventListener('mouseup', () => {
    window.removeEventListener('mousemove', resizeMove)
  })
}
</script>

<style scoped lang="less">
.ass-resize-item {
  position: relative;
  border: 1px solid #d9d9d9;
  width: 200px;

  .ass-resize-item-prefix {
    height: 4px;
    width: 100%;
    position: absolute;
    top: -2px;
    cursor: row-resize;
  }
}
</style>