<template>
  <div
    :class="[
      'el-row',
      { 'el-row--flex': type === 'flex' },
      justify !== 'start' && `is-justify-${justify}`,
      align !== 'top' && `is-align-${align}`,
    ]"
    :style="style"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "ElRow",
  props: {
    gutter: {
      type: Number,
      default: 0,
    },
    type: String,
    justify: {
      type: String,
      default: "start",
      valiator: (val) =>
        ["start", "end", "center", "space-around", "space-between"].includes(
          val,
        ),
    },
    align: {
      type: String,
      default: "top",
      valiator: (val) => ["top", "middle", "bottom"].includes(val),
    },
  },
  computed: {
    style() {
      const style = {};
      if (this.gutter) {
        style.marginLeft = -this.gutter / 2 + "px";
        style.marginRight = -this.gutter / 2 + "px";
      }
      return style;
    },
  },
};
</script>

<style lang="scss" scoped>
.el-row {
  &::before,
  &::after {
    display: block;
    content: "";
  }

  &::after {
    clear: both;
  }

  &--flex {
    &::before,
    &::after {
      display: none;
    }
    display: flex;
  }
}

.is-justify-end {
  justify-content: flex-end;
}

.is-justify-center {
  justify-content: center;
}

.is-justify-space-around {
  justify-content: space-around;
}

.is-justify-space-between {
  justify-content: space-between;
}

.is-align {
  justify-content: space-between;
}

.is-align-middle {
  align-items: center;
}

.is-align-bottom {
  align-items: flex-end;
}
</style>