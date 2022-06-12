<template>
  <div :class="['el-col', classList]" :style="style">
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
    offset: {
      type: Number,
      default: 0,
    },
    push: {
      type: Number,
      default: 0,
    },
    pull: {
      type: Number,
      default: 0,
    },
    xs: [Number, Object],
    sm: [Number, Object],
    md: [Number, Object],
    lg: [Number, Object],
    xl: [Number, Object],
  },
  computed: {
    gutter() {
      let parent = this.$parent;
      while (parent && parent.$options.name !== "ElRow") {
        parent = parent.$parent;
      }
      return parent ? parent.gutter : 0;
    },
    style() {
      const style = {};

      if (this.gutter) {
        style.paddingLeft = this.gutter / 2 + "px";
        style.paddingRight = style.paddingLeft;
      }
      return style;
    },
    classList() {
      const classList = [];
      ["span", "offset", "push", "pull"].forEach((prop) => {
        if (this[prop]) {
          classList.push(
            prop === "span"
              ? `el-col-${this[prop]}`
              : `el-col-${prop}-${this[prop]}`,
          );
        }
      });
      ["xs", "sm", "md", "lg", "xl"].forEach((size) => {
        if (typeof this[size] === "number") {
          classList.push(`el-col-${size}-${this[size]}`);
        } else if (typeof this[size] === "object") {
          const props = this[size];
          Object.keys(props).forEach((prop) => {
            classList.push(
              prop === "span"
                ? `el-col-${size}-${props[prop]}`
                : `el-col-${size}-${prop}-${props[prop]}`,
            );
          });
        }
      });
      return classList;
    },
  },
};
</script>

<style lang="scss">
@use "sass:map";
@import "@src/scss/breakpoints.scss";

.el-col {
  float: left;
  box-sizing: border-box;
}

@for $i from 0 through 24 {
  $val: calc($i / 24) * 100%;
  .el-col-#{$i} {
    width: $val;
  }

  .el-col-offset-#{$i} {
    margin-left: $val;
  }

  .el-col-push-#{$i} {
    position: relative;
    left: $val;
  }

  .el-col-pull-#{$i} {
    position: relative;
    right: $val;
  }
}

$list: (
  (
    "size": "xs",
    "prop": "max-width",
    "val": $sm - 1,
  ),
  (
    "size": "sm",
    "prop": "min-width",
    "val": $sm,
  ),
  (
    "size": "md",
    "prop": "min-width",
    "val": $md,
  ),
  (
    "size": "lg",
    "prop": "min-width",
    "val": $lg,
  ),
  (
    "size": "xl",
    "prop": "min-width",
    "val": $xl,
  )
);

@for $i from 1 through length($list) {
  $item: nth($list, $i);

  $prop: map.get($item, "prop");
  $val: map.get($item, "val");
  $size: map.get($item, "size");

  @media only screen and (#{$prop}: $val) {
    @for $i from 0 through 24 {
      $val: calc($i / 24) * 100%;
      
      .el-col-#{$size}-0 {
        display: none;
      }

      .el-col-#{$size}-#{$i} {
        width: $val;
      }

      .el-col-#{$size}-offset-#{$i} {
        margin-left: $val;
      }

      .el-col-#{$size}-push-#{$i} {
        position: relative;
        left: $val;
      }

      .el-col-#{$size}-pull-#{$i} {
        position: relative;
        right: $val;
      }
    }
  }
}
</style>