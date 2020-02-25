<template>
  <div class="home">
    <button
      class="home__add-item-button"
      @click="addItem">Add item</button>
    <AppChartPie :list="sliceData" />
  </div>
</template>

<script>
import AppChartPie from '@/components/AppChartPie.vue';

export default {
  name: 'Home',
  components: {
    AppChartPie,
  },
  data() {
    return {
      sliceData: [
        {
          id: 1,
          color: 'Coral',
          name: 'Test 1',
          count: 44,
        },
        {
          id: 2,
          color: 'CornflowerBlue',
          name: 'Test 2',
          count: 12,
        },
        {
          id: 3,
          color: '#00ab6b',
          name: 'Test 3',
          count: 73,
        },
      ],
    };
  },
  methods: {
    addItem() {
      this.sliceData.push({
        id: this.sliceData.length + 1,
        color: this.generateRandomColor(),
        name: `Test ${this.sliceData.length + 1}`,
        count: (Math.random() * 100),
      });
    },
    // Function from: https://css-tricks.com/snippets/javascript/random-hex-color/
    generateRandomColor() {
      return `#${Math.floor(Math.random() * 16777215).toString(16)}`;
    },
  },
};
</script>

<style lang="scss">
.home {
  &__add-item-button {
    margin-bottom: 24px;
  }
}
</style>
