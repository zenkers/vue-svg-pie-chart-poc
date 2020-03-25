<template>
  <ul class="app-chart-legend">
    <li
      v-for="item in list"
      :key="item.id"
      class="app-chart-legend__item">
      <div
        class="app-chart-legend__item-color"
        :style="{
          backgroundColor: item.color,
        }"></div>
      {{ item.name }}
      <input
        type="checkbox"
        :disabled="disableCheckbox && item.checked"
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
    disableCheckbox: {
      type: Boolean,
      required: false,
      default: false,
    },
    list: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  methods: {
    handleChange(item) {
      this.$emit('checkEvent', item);
    },
  },
};
</script>

<style lang="scss">
.app-chart-legend {
  padding: 0;
  display: flex;
  flex-direction: column;
  list-style: none;


  &__item {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    padding: 2px;
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
