<template>
  <div class="input__actions">
    <ButtonComponent icon="calendar"/>
    <SelectComponent/>
  </div>
</template>

<script>
import ButtonComponent from '@/components/@ui/ButtonComponent.vue';
import SelectComponent from '@/components/@ui/SelectComponent.vue';

export default {
  name: 'EditorInputActionsComponent',
  components: {
    ButtonComponent,
    SelectComponent,
  },
};
</script>

<style scoped lang="scss">
  .input__actions {
    margin: -6px;
    display: flex;
    align-items: flex-start;

    * {
      margin-left: 8px;

      &:first-child {
        margin-left: 0;
      }
    }
  }
</style>
