<template>
  <div class="index__wrapper">
    <EditorInput v-model="value"/>
  </div>
</template>

<script>
import EditorInput from '@/components/@ui/EditorInputComponent.vue';

export default {
  name: 'IndexPage',
  components: {
    EditorInput,
  },
  data() {
    return {
      value: '',
    };
  },
};
</script>

<style scoped lang="scss">
  .index__wrapper {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
