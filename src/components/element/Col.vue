<template>
  <div :class="['el-col', `el-col-${span}`]">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    span: {
      type: Number,
      default: 24,
    },
  },
};
</script>

<style lang="scss">
.el-col {
  float: left;
  width: 100%;
}
@for $i from 0 through 24 {
  .el-col-#{$i} {
    width: $i / 24 * 100%;
  }
}
</style>