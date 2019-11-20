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
        v-on:childToParent="onChildClick(...arguments)"
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
  methods: {
    onChildClick (response) {
      console.log(response)
      this.fromChild = response
      return response
    },
    setImage (imageSet) {
      let finalImage
      if (imageSet !== undefined) {
        finalImage = imageSet.images[0].source.url
        finalImage = finalImage.replace(/&amp;/g, '&')
      } else {
        finalImage = require('../static/placeholder.png')
      }
      return finalImage
    }
  },
}
</script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped lang="scss">

    @import "../scss/_variables.scss";

    .item-list {
        padding: 50px 20px;
        padding-top: 150px;
    }
  </style>
