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

<style lang="scss">
.app-chart-pie {
  h2 {
    margin: 6px 0;
  }

  &__svg {
    overflow: visible;
    filter: url('#fe-dropshadow');
  }

  &__path {
    transition: all .33s cubic-bezier(.33,.67,.47,1.18);

    &:hover {
      transform: scale(1.1);
    }
  }
}
</style>
