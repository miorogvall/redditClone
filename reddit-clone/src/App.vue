<template>
  <div id="app">
    <div id="previous">
        <p class="pagination-icon">←</p>
        <p class="pagination-text">previous 25</p>
    </div>
    <div id="item-list">
      <ItemList :posts="posts"/>
    </div>
    <div id="next">
        <p class="pagination-icon">→</p>
        <p class="pagination-text">next 25</p>
    </div>
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
        this.posts = response.data.data.children
      })
    }
}
</script>

<style lang="scss">

  * {
    box-sizing: border-box;
  }

  $black-color: #272727;
  $white: white;

html, body {
  min-height: 100vh;
  padding: 0;
  margin: 0;
  color: $black-color;
}

p {
  padding:0;
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  flex-direction: row;

  #previous, #next {
    height: 100vh;
    background: whitesmoke;
    width: 10%;
    max-width: 200px;
    min-width: 120px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 0 15px;
    transition: background 0.2s ease;

    &:hover {
      background: $black-color;
      cursor: pointer;
      color: $white;
    }

    .pagination-icon {
    }
  }

  #item-list {
    flex: 1 0 auto;
  }

}
</style>
