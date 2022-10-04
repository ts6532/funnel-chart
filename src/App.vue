<script setup lang="ts">
import FunnelChart from "./components/FunnelChart.vue";
</script>

<template>
  <div class="page">
    <FunnelChart :sections="2">
      <template #row-0>
        <div class="first-block"></div>
      </template>
      <template #row-1>
        <div class="first-block"></div>
      </template>
    </FunnelChart>
  </div>
</template>

<style>
.first-block {
  height: 20rem;
  background-color: green;
}

body {
  background-color: bisque;
  --page-margin: 1.5rem;
  --page-padding: 1.2rem;
}

.page {
  margin: var(--page-margin);
  padding: var(--page-padding);
  border-radius: 1rem;
  background-color: #ffffff;
  min-width: calc(100vw - var(--page-margin) * 2 - var(--page-padding) * 2);
  min-height: calc(100vh - var(--page-margin) * 2 - var(--page-padding) * 2);
}
</style>
