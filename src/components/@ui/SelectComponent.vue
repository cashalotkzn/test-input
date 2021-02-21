<template>
  <div class="custom__select">
    <span class="selected__option">
      No list
    </span>
  </div>
</template>

<script>
export default {
  name: 'SelectComponent',
};
</script>

<style scoped lang="scss">
  .custom__select {
    background-color: #F4F4F8;
    position: relative;
    padding: 8px 32px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    cursor: pointer;

    .selected__option {
      color: #46474A;
      font-size: .8rem;
    }

    &:before {
      content: '';
      display: block;
      position: absolute;
      width: 10px;
      height: 10px;
      box-sizing: border-box;
      left: 13px;
      top: 12px;
      border: 2px solid #C4C5C8;
      border-radius: 4px;
    }

    &:after {
      content: '';
      display: block;
      position: absolute;
      width: 10px;
      height: 10px;
      right: 12px;
      background-image: url('../../assets/icons/ArrowDown.svg');
      background-position: center;
      background-size: contain;
    }
  }
</style>
