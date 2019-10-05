<template>
    <div class="item-list">
      <h1>this is the item list</h1>
      <Item v-for="post in posts" :title="post.data.author"/>
    </div>
</template>
  <script>

import Item from '@/components/Item.vue'

export default {
  name: 'ItemList',
  components: {
    Item
  },
  props: ['posts'],
  created: function () {
    console.log('user data from parent component:')
    console.log(this.posts) //prints out an empty string
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
