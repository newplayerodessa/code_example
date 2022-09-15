<template>
  <div id="app">
      <cards-viewer/>
  </div>
</template>

<script>
import CardsViewer from "@/components/cards/cards-viewer";
export default {
  name: 'App',
  components: {
    CardsViewer
  }
}
</script>

<style lang="scss">
#app {}
</style>
