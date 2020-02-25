<template>
  <div class="chart-pie">
    <svg
      viewBox="-1 -1 2 2"
      class="chart-pie__chart">
      <template v-for="item in listRendered">
        <path
          v-if="item.checked"
          :key="item.id"
          :fill="item.color"
          :d="item.d" />
      </template>
    </svg>

    <div class="chart-pie__legend">
      <AppChartLegend
        :list="listCalculated"
        @checkedEvent="handleCheckedEvent" />
    </div>
  </div>
</template>


<script>
import AppChartLegend from '@/components/v1/AppChartLegend.vue';

export default {
  name: 'AppChartPie',
  components: {
    AppChartLegend,
  },
  props: {
    list: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  data() {
    return {
      listActive: this.list.map((item) => ({ ...item, checked: true })),
    };
  },
  watch: {
    list() {
      this.listActive = this.list.map((item) => ({ ...item, checked: true }));
    },
  },
  computed: {
    /**
     * Adds { percent, d } to items that ARE CHECKED (for svg rendering)
     * Adds { percent: null } to items that are unchecked (to overwrite if set prior)
     * Returns a list of items with properties required for graph and legend templates
     */
    listCalculated() {
      let cumulativePercent = 0;
      const newList = [];

      // Sum of the count property for checked items
      const sumCheckedItemCount = this.listActive.reduce((sum, item) => {
        if (item.checked) return sum + item.count;
        return sum;
      }, 0);

      this.listActive.forEach((item) => {
        if (item.checked) {
          // Math from: https://medium.com/hackernoon/a-simple-pie-chart-in-svg-dbdd653b6936
          const [startX, startY] = this.getCoordinatesForPercent(cumulativePercent);
          const percent = item.count / sumCheckedItemCount;
          const largeArcFlag = percent > 0.5 ? 1 : 0;
          cumulativePercent += percent;
          const [endX, endY] = this.getCoordinatesForPercent(cumulativePercent);

          newList.push({
            ...item,
            percent,
            d: this.formatDrawnPath(startX, startY, largeArcFlag, endX, endY),
          });
        } else {
          newList.push({ ...item, percent: null }); // set percent to overwrite
        }
      });

      return newList;
    },
    listRendered() {
      return this.listCalculated.filter((item) => item.checked);
    },
  },
  methods: {
    formatDrawnPath(startX, startY, largeArcFlag, endX, endY) {
      return [
        `M ${startX} ${startY}`, // Move
        `A 1 1 0 ${largeArcFlag} 1 ${endX} ${endY}`, // Arc
        'L 0 0', // Line
      ].join(' ');
    },
    getCoordinatesForPercent(percent) {
      const x = Math.cos(2 * Math.PI * percent);
      const y = Math.sin(2 * Math.PI * percent);
      return [x, y];
    },
    handleCheckedEvent(data) {
      const index = this.listActive.findIndex((item) => item.id === data.id);
      if (index !== -1) {
        this.listActive.splice(index, 1); // Remove item...
        this.listActive.splice(index, 0, data); // re-insert for re-render
      }
    },
  },
};
</script>

<style lang="scss">
.chart-pie {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

  &__chart {
    height: 400px;
    width: 400px;
    transform: rotate(-90deg)
  }
}
</style>
