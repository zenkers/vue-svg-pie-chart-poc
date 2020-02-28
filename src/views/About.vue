<template>
  <div class="about">
    <!-- Add item button -->
    <button
      class="about__add-item-button"
      @click="AddListItem">
      Add item
    </button>

    <!-- Diagram -->
    <AboutDiagram :list="list" />
  </div>
</template>

<script>
import AboutDiagram from '@/components/v2/AboutDiagram.vue';

export default {
  name: 'About',
  components: {
    AboutDiagram,
  },
  data() {
    return {
      list: [
        {
          id: 1,
          name: 'Test 1',
          count1: 44,
          count2: 82,
          count3: 13,
          count4: 1,
        },
        {
          id: 2,
          name: 'Test 2',
          count1: 12,
          count2: 44,
          count3: 10,
          count4: 3,
        },
        {
          id: 3,
          name: 'Test 3',
          count1: 73,
          count2: 14,
          count3: 55,
          count4: 5,
        },
      ],
    };
  },
  methods: {
    AddListItem() {
      this.list.push({
        id: this.list.length + 1,
        name: `Test ${this.list.length + 1}`,
        count1: (Math.random() * 100),
        count2: (Math.random() * 100),
        count3: (Math.random() * 100),
        count4: (Math.random() * 100),
      });
    },
  },
};
</script>
