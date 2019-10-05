<template>
  <div id="app">
      <ItemList/>
      <ul>
        <li v-for="post in posts">{{post.data.author}}</li>

      </ul>
  </div>
</template>

<script>
  // @ is an alias to /src
  import ItemList from '@/components/ItemList.vue'
  import Item from '@/components/Item.vue'
  import axios from 'axios'

  export default {
    name: 'App',
    components: {
      ItemList
    },
    data() {
      return { posts: [] }
    },
  created () {
    axios
      .get('https://www.reddit.com/r/aww.json?limit=10')
      .then(response => {
        this.posts = response.data.data.children;
      })
    }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: 100;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
