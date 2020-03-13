<template>
  <div class="demo-diagram">
    <div class="demo-diagram__charts">
      <template v-for="(list, index) in renderLists">
        <AppChartPie
          :key="index"
          :list="list"
          :title="`Count ${index + 1}`" />
      </template>
    </div>
    <AppChartLegend
      :list="listActive"
      @checkEvent="handleCheckEvent" />
  </div>
</template>

<script>
import AppChartLegend from '@/components/AppChartLegend.vue';
import AppChartPie from '@/components/AppChartPie.vue';

export default {
  name: 'Demo',
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
      renderLists: [],
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
        // Originated from: https://medium.com/hackernoon/a-simple-pie-chart-in-svg-dbdd653b6936
        const [startX, startY] = this.getCoordinatesForPercent(cumulativePercent);
        const percent = item.checked ? (item.count / sumCheckedItemCount) : 0;
        const largeArcFlag = percent > 0.5 ? 1 : 0;
        cumulativePercent += percent;
        const [endX, endY] = item.checked
          ? this.getCoordinatesForPercent(cumulativePercent)
          : [startX, startY];

        newList.push({
          ...item,
          percent,
          d: this.formatDrawnPath(startX, startY, largeArcFlag, endX, endY),
        });
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
    generateRandomColor() {
      // https://css-tricks.com/snippets/javascript/random-hex-color/
      let color = `#${Math.floor(Math.random() * 16777215).toString(16)}`;
      // Ensures full hex (above snippet inconsistent)
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
      this.renderLists = [];
      this.renderLists.push(this.calcListPercent(this.formatList('count1')));
      this.renderLists.push(this.calcListPercent(this.formatList('count2')));
      this.renderLists.push(this.calcListPercent(this.formatList('count3')));
      this.renderLists.push(this.calcListPercent(this.formatList('count4')));
    },
  },
};
</script>

<style lang="scss">
.demo-diagram {
  margin: 0 auto;
  max-width: 400px;
  display: flex;
  flex-direction: row;

  &__charts {
    margin-right: 16px;
    flex-grow: 1;
    display: grid;
    grid-gap: 16px;
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
