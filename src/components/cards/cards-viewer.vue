<template>
  <div class="items">
    <div class="container">
      <div class="items__controls">
        <cards-filters
            name="Selected filters:"
            @checkedValues="checkedValues"
        />
        <cards-picker name="toggle-picker">
          <button
              @click="pricePicker('cheap')"
              :class="{active: this.sortedByPrice === 'cheap'}"
              class="btn-picker"
          >By Cheap
          </button>
          <button
              @click="pricePicker('expansive')"
              :class="{active: this.sortedByPrice === 'expansive'}"
              class="btn-picker"
          >By Expansive
          </button>
        </cards-picker>
      </div>

    </div>
    <div class="container">
      <h1 class="items__title">Cards viewer <span class="items__count">{{selectedItems.length}}</span></h1>
      <div class="items__wrap">
        <article
            v-for="(card,index) in selectedItems"
            :key="index"
            class="item"
        >
          <div class="item__header">
            <div class="item__image">
              <img :src="card.image" alt="">
            </div>
          </div>
          <div class="item__body">
            <p class="item__name">{{card.name}}</p>
            <p class="item__description">{{card.description}}</p>
            <ul class="item__sizes">
              <span>Available sizes:</span>
              <li
                  v-for="(size,index) in card.sizes"
                  :key="index"
              >
                {{size}}
              </li>
            </ul>
            <ul class="item__colors">
              <span>Available colors:</span>
              <li
                  v-for="(color,index) in card.colors"
                  :key="index"
              >

                {{color}}
              </li>
            </ul>
            <p class="item__price">Price: <b>{{card.price}}</b></p>
          </div>
        </article>
      </div>
    </div>
  </div>
</template>

<script>
import cards from "@/items/cards";
import CardsFilters from '@/components/filters/cards-filters'
import CardsPicker from '@/components/filters/cards-picker'
export default {
  name: "cards-viewer",
  components: {CardsPicker, CardsFilters},
  data() {
    return {
      sortedByPrice: null,
      cards: cards,
      applyFilters: []
    }
  },
  computed: {
    selectedItems() {
      return this.applyFilters.length > 0 ? this.applyFilters : this.cards;
    },

  },
  methods: {
    resetPrice() {
      this.sortedByPrice = null;
    },
    pricePicker(settings) {
      this.sortedByPrice = settings;
    },
    checkedValues(items) {
     if (items && this.applyFilters) {
       this.pricePicker(this.sortedByPrice)
       let choosen = this.cards.filter((el) => {
         return el.keywords.includes(...items)
       })
       this.applyFilters = choosen;
     }
    }
  },
  watch: {
    sortedByPrice() {
      this.selectedItems.sort((prev,curr) => {
        if (this.sortedByPrice === 'cheap') {
          return prev.price - curr.price;
        } else {
          return curr.price - prev.price;
        }
      })
    }
  }

}
</script>


<style lang="scss" scoped>

.items {

  &__title {
    @include _h1-primary;
  }

  &__wrap {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-gap: 10px;
    margin-bottom: 40px;

    @include _sm {
      grid-template-columns: repeat(2, 1fr);
    }

    @include _md{
      grid-template-columns: repeat(3, 1fr);
    }

    @include _lg {
      grid-template-columns: repeat(4, 1fr);
    }
  }
}

.item {
  display: flex;
  flex-direction: column;
  background: $main-white;
  border: solid 1px $border-color;
  border-radius: $el-radius;
  box-shadow: $light-shadow;

  ul {
    list-style: none;
    list-style-type: none;
    padding: 0;
  }

  &__header {
    height: 200px;
    margin: 0 0 16px 0;
  }

  &__body {
    display: flex;
    flex-direction: column;
    padding: 0 8px;
    height: 100%;
  }

  &__image {
    height: 100%;
    background: $primary-grey;

    img {
      display: block;
      margin: auto;
      max-width: 100%;
      object-fit: cover;
      height: inherit;
    }
  }

  &__name {
    flex: 0;
  }

  &__price {
    flex: 0;
  }

  &__description {
    @include _item-description;
    flex: 1 0;
    height: 100%;

  }

  &__name, &__price {
    @include _item-name;
    margin: 0 0 16px 0;
  }

  &__sizes, &__colors {
    display: block;
    margin: 0 0 16px 0;

    span {
      margin: 0 16px 0 0;
    }

    li {
      display: inline-block;
      text-align: center;
      min-width: 48px;
      margin: 0 8px 8px 0;
      padding: 4px 8px;
      background: $primary-grey;
      border-radius: $btns-radius;
    }
  }
}

</style>