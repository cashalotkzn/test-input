<template>
  <button class="custom__button">
    <SVGIcon :icon="icon" color="#636773" :size="16"/>
  </button>
</template>

<script>
import SVGIcon from '@/components/@ui/SVGIcon.vue';

export default {
  name: 'ButtonComponent',
  props: {
    icon: {
      type: String,
      required: true,
    },
  },
  components: {
    SVGIcon,
  },
};
</script>

<style scoped lang="scss">
  .custom__button {
    background-color: #F2F3F6;
    padding: 9px;
    border-radius: 10px;
    outline: none;
    border: none;
    display: flex;
    cursor: pointer;
  }
</style>
