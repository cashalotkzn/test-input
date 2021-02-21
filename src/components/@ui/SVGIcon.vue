<template>
  <component :is="icon" :style="style"/>
</template>

<script>
import Calendar from '@/components/icons/CalendarIconComponent.vue';

export default {
  name: 'SVGIcon',
  components: {
    Calendar,
  },
  props: {
    icon: {
      type: String,
      required: true,
    },
    size: {
      type: Number,
      default: 20,
    },
    color: {
      type: String,
      default: '#000000',
    },
  },
  computed: {
    style() {
      return {
        width: this.size,
        height: this.size,
        color: this.color,
      };
    },
  },
};
</script>
