<template>
  <div class="app-chart-pie">
    <h2 v-if="title">
      {{ title }}
    </h2>
    <svg
      viewBox="-1 -1 2 2"
      class="app-chart-pie__svg">
      <template v-for="item in list">
        <g :key="item.id" class="app-chart-pie__group">
          <path
            :fill="item.color"
            :d="item.d"
            class="app-chart-pie__shadow">
          </path>
          <path
            :fill="item.color"
            :d="item.d"
            class="app-chart-pie__path">
          </path>
        </g>
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
  }

  &__group {
    &:hover > .app-chart-pie__path {
      transform: scale(1.1);
    }

    &:hover > .app-chart-pie__shadow {
      transform: scale(1.175);
    }
  }

  &__path {
    transition: all .33s cubic-bezier(.33,.67,.47,1.18);
  }

  &__shadow {
    opacity: 0.5;
    transform: scale(.80);
    transition: all .37s cubic-bezier(.33,.67,.47,1.18);
  }
}
</style>
