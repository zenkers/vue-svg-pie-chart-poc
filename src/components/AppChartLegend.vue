<template>
  <ul class="chart-legend">
    <li
      v-for="slice in slices"
      :key="slice.id"
      class="chart-legend__item">
      <div
        class="chart-legend__item-color"
        :style="{
          backgroundColor: slice.color,
        }"></div>
      {{ slice.name }}
      <input
        type="checkbox"
        :id="`checkbox-${slice.id}`"
        v-model="slice.checked"
        @change="handleChange(slice)">
    </li>
  </ul>
</template>

<script>
export default {
  name: 'AppChartLegend',
  props: {
    slices: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  methods: {
    handleChange(slice) {
      this.$emit('checkedEvent', slice);
    },
  },
};
</script>

<style lang="scss">
.chart-legend {
  width: 100%;
  list-style: none;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;

  &__item {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 6px;
  }

  &__item-color {
    display: inline-block;
    width: 24px;
    height: 24px;
    margin-right: 6px;
    border: 1px solid #000;
  }
}
</style>
