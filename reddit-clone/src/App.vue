<template>
  <div id="app">
      <ItemList/>
  </div>
</template>

<script>
    // @ is an alias to /src
    import ItemList from '@/components/ItemList.vue'
    import Item from '@/components/Item.vue'
    
    export default {
      name: 'App',
      components: {
        ItemList
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
