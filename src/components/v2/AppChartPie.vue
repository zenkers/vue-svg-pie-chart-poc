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
          v-if="item.checked"
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
    margin: 0px;
  }

  &__svg {
    overflow: hidden;

    &:hover {
      overflow: visible;
    }
  }

  &__path {
    // transition: all .2s linear;
    transition: all .2s ease-in-out;
    // transition: all .25s cubic-bezier(0, 0, 0.07, 1.06);
    // transition: all .25s cubic-bezier(0, 0.7, 0.6, 1.04);
    //cubic-bezier(0.19, 0.43, 0, 1.21)

    &:hover {
      transform: scale(1.115);
    }
  }
}
</style>
