<template>
  <div class="app-chart-pie">
    <h2 v-if="title">
      {{ title }}
    </h2>
    <svg
      viewBox="-1 -1 2 2"
      class="app-chart-pie__svg">
      <template v-for="item in list">
        <path
          v-if="item.checked"
          :key="item.id"
          :fill="item.color"
          :d="item.d"
          class="app-chart-pie__path">
          </path>
      </template>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'AppPieChart',
  props: {
    list: {
      type: Array,
      required: true,
      default: () => [],
    },
    title: {
      type: String,
      required: false,
      default: '',
    },
  },
};
</script>
