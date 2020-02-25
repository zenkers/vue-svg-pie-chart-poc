<template>
  <ul class="chart-legend">
    <li
      v-for="item in list"
      :key="item.id"
      class="chart-legend__item">
      <div
        class="chart-legend__item-color"
        :style="{
          backgroundColor: item.color,
        }"></div>
      {{ item.name }}
      <template v-if="item.percent">
        ({{ (item.percent * 100).toFixed(2) }}%)
      </template>
      <input
        type="checkbox"
        :id="`checkbox-${item.id}`"
        v-model="item.checked"
        @change="handleChange(item)">
    </li>
  </ul>
</template>

<script>
export default {
  name: 'AppChartLegend',
  props: {
    list: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  methods: {
    handleChange(item) {
      this.$emit('checkedEvent', item);
    },
  },
};
</script>

<style lang="scss">
.chart-legend {
  width: 100%;
  padding: 0;
  display: grid;
  grid-gap: 6px;
  grid-template-columns: repeat(3, 1fr);
  list-style: none;


  &__item {
    padding: 6px;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 6px;
    background-color: #585858;
    border: 1px solid #777;
  }

  &__item-color {
    display: inline-block;
    width: 24px;
    height: 24px;
    border: 1px solid #000;
  }
}
</style>
