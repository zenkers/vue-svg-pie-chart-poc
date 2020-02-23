<template>
  <div class="chart-pie">
    <svg
      viewBox="-1 -1 2 2"
      class="chart-pie__chart">
      <path
        v-for="slice in formattedSlices"
        :key="slice.id"
        :fill="slice.color"
        :d="slice.d" />
    </svg>

    <div v-if="showLegend" class="chart-pie__legend">
      <AppChartLegend :slices="formattedSlices" />
    </div>
  </div>
</template>

<script>
import AppChartLegend from '@/components/AppChartLegend.vue';

/**
 * Logic heavily influenced by the following article:
 * https://medium.com/hackernoon/a-simple-pie-chart-in-svg-dbdd653b6936
 */

export default {
  name: 'AppChartPie',
  components: {
    AppChartLegend,
  },
  props: {
    showLegend: {
      type: Boolean,
      required: false,
      default: true,
    },
    slices: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  computed: {
    formattedSlices() {
      let cumulativePercent = 0;
      const sumCount = this.slices.reduce((sum, slice) => sum + slice.count, 0);
      const formatted = [];

      this.slices.forEach((slice) => {
        const [startX, startY] = this.getCoordinatesForPercent(cumulativePercent);
        const percent = slice.count / sumCount;
        const largeArcFlag = percent > 0.5 ? 1 : 0;

        cumulativePercent += percent;

        const [endX, endY] = this.getCoordinatesForPercent(cumulativePercent);

        formatted.push({
          ...slice,
          percent,
          d: this.formatDrawnPath(startX, startY, largeArcFlag, endX, endY),
        });
      });

      return formatted;
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

  &__legend {
    max-width: 360px;
  }
}
</style>
