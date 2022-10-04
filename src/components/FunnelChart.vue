<script setup lang="ts">
import { computed } from "vue";
import { onMounted, onUnmounted, reactive } from "vue";
import { ref } from "vue";
import FunnelChartBlock from "./FunnelChartBlock.vue";

type FunnelProps = {
  minWidth?: number;
  maxWidth?: number;
  ratio?: number;
  sections: number;
};

const props = withDefaults(defineProps<FunnelProps>(), {
  minWidth: 560, // ширина при которой угол наклона равен 0
  maxWidth: 1280, // ширирна при которой угол наклона достигает заданного значения через ratio
  ratio: 0.03, // коэфициент на который умножаем высоту блока для расчета угла наклона
});

const screenWidth = ref(0);

const baseElem = document.querySelector("html");
const remRation = computed(() => {
  return Number(baseElem?.style.fontSize) || 16;
});

const firstElem = ref<HTMLElement>();
const rows = ref(
  [...Array(props.sections)].map((e, i) => {
    return reactive({
      _id: i,
      sectionWidth: 0,
      seWidth: 0,
      seHeight: 0,
    });
  })
);

const itemRefs = ref<Element[]>([]);

const toRem = (value: number) => {
  return value / remRation.value;
};

const calcBlocksWidth = () => {
  if (!firstElem.value) return;

  screenWidth.value = window.innerWidth;

  const ratio =
    screenWidth.value > props.maxWidth ? props.ratio * 2 : props.ratio;

  let seWidthFactor =
    ((screenWidth.value - props.minWidth) * ratio) /
    (props.maxWidth - props.minWidth);

  let edgeFactor = seWidthFactor > 0 ? seWidthFactor : 0;

  for (const row of rows.value) {
    const prevRowWidth =
      row._id === 0
        ? toRem(firstElem.value.clientWidth)
        : rows.value[row._id - 1].sectionWidth;

    const currentElement = itemRefs.value[row._id];

    const prevRow = rows.value[row._id - 1];

    row.seHeight = toRem(currentElement.clientHeight);
    row.seWidth = toRem(currentElement.clientHeight) * edgeFactor;
    row.sectionWidth =
      row._id === 0 ? prevRowWidth : prevRowWidth - prevRow.seWidth * 2;
  }
};

window.addEventListener("resize", calcBlocksWidth, false);

window.onload = calcBlocksWidth;

onMounted(() => {
  calcBlocksWidth();
});

onUnmounted(() => {
  window.removeEventListener("resize", calcBlocksWidth, false);
});
</script>

<template>
  <div class="funnel-chart" ref="firstElem"></div>

  <template v-for="row in rows" :key="row._id">
    <div
      class="funnel-chart__section"
      :style="{ width: `${row.sectionWidth}rem` }"
    >
      <FunnelChartBlock :width="row.seWidth" :height="row.seHeight">
        <div
          class="funnel-chart__section-content"
          :ref="(el) => itemRefs.push(el as Element)"
        >
          <slot :name="`row-${row._id}`"></slot>
        </div>
      </FunnelChartBlock>
    </div>
  </template>
</template>

<style>
.funnel-chart {
  width: 100%;
}

.funnel-chart__section {
  position: relative;
  margin: 0 auto;
  margin-top: 2rem;
}

.funnel-chart__section-content {
  width: 100%;
  height: min-content;
}
</style>
