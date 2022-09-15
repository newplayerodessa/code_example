<template>
  <div :class="name">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "cards-picker",
  props: {
    name: String
  }

}
</script>

<style lang="scss" scoped>
  .toggle-picker {
    .btn-picker {
      background: $main-white;
      margin: 0 16px 8px 0;
      padding: 10px 25px;
      font-size: 1rem;
      border: solid 1px $border-color;
      border-radius: $btns-radius;
      cursor: pointer;

      &:last-child {
        margin-right: 0;
      }

      &.active {
        background: $accent-color;
        color: #fff;
      }
    }
  }

</style>