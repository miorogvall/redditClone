<template>
    <div class="item-list">
      <h1>this is the item list</h1>
      <Item title="this is the title"/>
    </div>
</template>
  <script>

import Item from '@/components/Item.vue'

export default {
  name: 'ItemList',
  components: {
    Item
  }
}
  </script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped lang="scss">
.item-list {
    div {
        color:red;
    }
}
  </style>
