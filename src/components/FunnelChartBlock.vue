<script setup lang="ts">
type FunnelBlockProps = {
  width: number;
  height: number;
};

defineProps<FunnelBlockProps>();
</script>

<template>
  <div
    class="funnel-chart-block funnel-chart-block--left"
    :style="{
      width: `${width}rem`,
      height: `${height}rem`,
      borderLeftWidth: `${width}rem`,
      borderTopWidth: `${height}rem`,
    }"
  ></div>

  <slot />

  <div
    class="funnel-chart-block funnel-chart-block--right"
    :style="{
      width: `${width}rem`,
      height: `${height}rem`,
      borderRightWidth: `${width}rem`,
      borderTopWidth: `${height}rem`,
    }"
  ></div>
</template>

<style>
.funnel-chart-block {
  border-top: solid;
  border-top-color: transparent;
  position: absolute;
  top: 0;
}

.funnel-chart-block--left {
  border-left: solid;
  border-left-color: white;
  left: 0;
}

.funnel-chart-block--right {
  border-right: solid;
  border-right-color: white;
  right: 0;
}
</style>
