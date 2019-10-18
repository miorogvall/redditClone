<template>
    <div class="item-list">
      <Item
        v-for="post in posts"
        v-bind:key="post.id"
        :thumbnail="setImage(post.data.preview)"
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
        <postModal :dataRecieved="fromChild" :isOpen="modalOpen" @isOpen="closeModal"/>
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
    posts: Array,
  },
  data () {
    return {
      fromChild: this.fromChild,
      modalOpen: 'is-open'
    }
  },
  methods: {
    onChildClick (value) {
      this.fromChild = value
      var document = new DOMParser().parseFromString(value.data.selftext_html, "text/html");
      console.log(document.body.textContent)
      value.data.selftext_html = document.body.textContent
      console.log(value)
      return value;
    },
    setImage (imageSet) {
      let finalImage
      console.log(imageSet)
      if(imageSet !== undefined) {
        finalImage = imageSet.images[0].source.url
      } else {
        finalImage = require('../static/placeholder.png')
      }
      return finalImage
    },
    closeModal () {
      this.isOpen = 'is-closed'
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
