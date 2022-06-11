<template>
  <div class="el-row">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    gutter: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style lang="scss" scoped>
.el-row {
  // &::before,
  &::after {
    display: block;
    content: "";
  }

  &::after {
    clear: both;
  }
}
</style>