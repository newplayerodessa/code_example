<template>
  <div class="filters">
    <div class="filters__wrap">
      <div class="filters__title">
        <h3 class="filters__description">{{ name }}</h3>
      </div>
      <div class="filters__cheaps">
        <span class="filters__choosen"
              v-for="(item,index) in choosenFilters"
              :key="index">
        {{item}}
      </span>
      </div>
    </div>
    <div class="filter">
      <div class="filter__wrap">
        <div class="filter__item">
          <div class="filter__picker"></div>
          <label
              for="sizeFilter"
              class="filter__title"
              data-filter="size-filter">
            Size Filter
            <!--          <span class="filter__arrow"></span>-->
          </label>
          <input
              v-model="isSize"
              type="checkbox"
              class="filter__handler"
              id="sizeFilter">
          <ul
              v-if="isSize"
              class="filter__dropdown">
            <li
                v-for="(size, index) in sizes"
                :key="index"
                @click="checkAttr(size)"
            >
              {{size}}
              <span class="filter__remove" v-if="checked.includes(size)"></span>
            </li>
          </ul>
        </div>
        <div class="filter__item">
          <div class="filter__picker"></div>
          <label
              for="colorFilter"
              ref="color-filter"
              class="filter__title">Color Filter
          </label>
          <input
              v-model="isColor"
              type="checkbox"
              class="filter__handler"
              id="colorFilter">
          <ul
              v-if="isColor"
              class="filter__dropdown">
            <li
                v-for="(color, index) in colors"
                :key="index"
                @click="checkAttr(color)"
            >
              {{color}}
              <span class="filter__remove" v-if="checked.includes(color)"></span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "cards-filters",
  props: {
    name: String,
  },
  data() {
    //fetch filters from api in future
    return {
      pricePicker: null,
      isSize: false,
      isColor: false,
      isPrice: false,
      checked: [],
      sizes: ['xl', 'l', 'm'],
      colors: ['red', 'green', 'blue'],
    }
  },
  computed: {
    choosenFilters() {
      return this.checked;
    }
  },
  methods: {
    checkAttr(item) {
      const index = this.checked.indexOf(item)
      if (index != -1) {
        this.checked.splice(index, 1)
      }
      else {
        this.checked.push(item)
      }
      this.$emit('checkedValues', this.checked)
    }
  }
}
</script>

<style lang="scss" scoped>
  .filters {
    &__choosen {
      display: inline-flex;
      flex-wrap: wrap;
      justify-content: center;
      margin-right: 8px;
      padding: 5px 10px;
      background: $main-white;
      border: solid 1px $border-color;
      border-radius: 24px;
      font-size: 0.8rem;
      color: $text-primary;
      text-transform: uppercase;
      font-weight: 600;
    }

    &__description {
      display: flex;
      align-items: center;
      white-space: nowrap;
      margin-right: 16px;
    }

    &__wrap {
      display: flex;
      align-items: center;

    }
    &__cheaps {
      flex: auto;
      white-space: nowrap;
      overflow-x: auto;
    }
  }

  .filter {
    &__wrap {
      display: flex;
    }

    &__item {
      position: relative;
      flex: 1px 0 128px;
    }

    &__title {
      position: relative;
      display: inline-flex;
      min-width: 120px;
      margin: 0 16px 16px 0;
      padding: 5px 16px;
      background: $main-white;
      border: solid 1px $border-color;
      border-radius: $btns-radius;
      cursor: pointer;

      &:last-child {
        margin-right: 0;
      }
    }

    &__handler {
      display: none;
      visibility: hidden;
      opacity: 0;
    }

    &__dropdown {
      position: absolute;
      z-index: 1;
      width: 100%;
      margin: 0;
      padding: 0;
      background: $main-white;
      border-radius: $el-radius;
      list-style: none;

      &:before {
        position: absolute;
        display: block;
        top: -10px;
        left: 0;
        width: 0;
        height: 0;
        border-style: solid;
        border-width: 0 12px 16px 12px;
        border-color: transparent transparent $main-white transparent;
        content: '';
      }

      li {
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 42px;
        margin: 0;
        padding: 5px 10px;
        border-bottom: solid 1px $border-color;
        font-size: 0.8rem;
        text-transform: uppercase;
        cursor: pointer;

        &:last-child {
          border-bottom: 0;
        }
      }
    }

    &__remove {
      position: relative;
      display: inline-flex;
      justify-content: center;
      align-items: center;
      width: 24px;
      height: 24px;
      margin-left: 16px;
      color: $main-white;
      background: $accent-color;
      border-radius: 50%;

      &:before {
        position: absolute;
        font-size: 22px;
        display: flex;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        content: '\00D7';
      }
    }
  }
</style>