<template>
    <div class="item-list">
      <Item
        v-for="post in posts"
        v-bind:key="post.id"
        :thumbnail="post.data.preview.images[0].source.url"
        :title="post.data.title"
        :commentNumber="post.data.num_comments"
        :author="post.data.author"
        :score="post.data.score"
        :permalink="post.data.permalink"
        :created="post.data.created_utc"
        :id="post.data.id"
        :parentData="post"
        v-on:childToParent="onChildClick(post)"
      />
        <postModal :dataRecieved="fromChild"/>
    </div>
</template>
  <script>

import Item from '@/components/Item.vue'
import postModal from '@/components/postModal.vue'

export default {
  name: 'ItemList',
  components: {
    Item,
    postModal
  },
  props: {
    posts: Array
  },
  data () {
    return {
      fromChild: this.fromChild
    }
  },
  computed: {
      toggleModal() {
        this.isOpen = true;
      }

  },
  methods: {
    onChildClick (value) {
      this.fromChild = value
      console.log('onChildClick')
      console.log(this.fromChild)
      console.log(value)
      return value;
    }
  }
}
  </script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped lang="scss">
.item-list {
    padding: 50px 20px;
}
  </style>
