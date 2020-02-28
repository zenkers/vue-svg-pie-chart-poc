<template>
  <div class="about-diagram">
    <div class="about-diagram__charts">
      <AppChartPie :list="listCount1" title="Count 1" />
      <AppChartPie :list="listCount2" title="Count 2" />
      <AppChartPie :list="listCount3" title="Count 3" />
      <AppChartPie :list="listCount4" title="Count 4" />
    </div>
    <AppChartLegend
      :list="listActive"
      @checkEvent="handleCheckEvent" />
  </div>
</template>

<script>
import AppChartLegend from '@/components/v2/AppChartLegend.vue';
import AppChartPie from '@/components/v2/AppChartPie.vue';

export default {
  name: 'AboutDiagram',
  components: {
    AppChartLegend,
    AppChartPie,
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
      listActive: [],
      listCount1: [],
      listCount2: [],
      listCount3: [],
      listCount4: [],
    };
  },
  watch: {
    list() {
      this.initializeListActive();
    },
  },
  created() {
    this.initializeListActive();
  },
  methods: {
    calcListPercent(list) {
      let cumulativePercent = 0;
      const newList = [];

      // Sum of the count property for checked items
      const sumCheckedItemCount = list.reduce((sum, item) => {
        if (item.checked) return sum + item.count;
        return sum;
      }, 0);

      list.forEach((item) => {
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
    formatDrawnPath(startX, startY, largeArcFlag, endX, endY) {
      return [
        `M ${startX} ${startY}`, // Move
        `A 1 1 0 ${largeArcFlag} 1 ${endX} ${endY}`, // Arc
        'L 0 0', // Line
      ].join(' ');
    },
    formatList(propertyName) {
      return this.listActive.map((item) => ({
        id: item.id,
        name: item.name,
        checked: item.checked,
        color: item.color,
        count: item[propertyName],
      }));
    },
    // Function partially from: https://css-tricks.com/snippets/javascript/random-hex-color/
    generateRandomColor() {
      let color = `#${Math.floor(Math.random() * 16777215).toString(16)}`;
      while (color.length < 7) {
        color = color.concat('', (Math.random() * 10).toFixed(0));
      }
      return color;
    },
    getCoordinatesForPercent(percent) {
      const x = Math.cos(2 * Math.PI * percent);
      const y = Math.sin(2 * Math.PI * percent);
      return [x, y];
    },
    handleCheckEvent(data) {
      const index = this.listActive.findIndex((item) => item.id === data.id);
      if (index !== -1) {
        this.listActive.splice(index, 1, data);
      }
      this.initializeListCount();
    },
    initializeListActive() {
      this.listActive = this.list.map((item) => ({
        ...item,
        checked: true,
        color: this.generateRandomColor(),
      }));
      this.initializeListCount();
    },
    initializeListCount() {
      this.listCount1 = this.calcListPercent(this.formatList('count1'));
      this.listCount2 = this.calcListPercent(this.formatList('count2'));
      this.listCount3 = this.calcListPercent(this.formatList('count3'));
      this.listCount4 = this.calcListPercent(this.formatList('count4'));
    },
  },
};
</script>

<style lang="scss">
.about-diagram {
  margin: 0 auto;
  max-width: 400px;
  display: flex;
  flex-direction: row;

  &__charts {
    margin-right: 12px;
    flex-grow: 1;
    display: grid;
    grid-gap: 12px;
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
