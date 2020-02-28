<template>
  <div class="contact">
    <!-- Add item button -->
    <button
      class="contact__add-item-button"
      @click="AddListItem">
      Add item
    </button>

    <!-- Diagram -->
    <ContactDiagram :list="list" />
  </div>
</template>

<script>
import ContactDiagram from '@/components/ContactDiagram.vue';

export default {
  name: 'contact',
  components: {
    ContactDiagram,
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
        }, {
          id: 4,
          name: 'Test 4',
          count1: 26,
          count2: 39,
          count3: 67,
          count4: 16,
        }, {
          id: 5,
          name: 'Test 5',
          count1: 68,
          count2: 49,
          count3: 18,
          count4: 14,
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
