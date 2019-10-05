<template>
    <div class="item">
      <h1>{{ title }}</h1>
    </div>
</template>
  <script>
  export default {
    name: 'Item',
    props: {
      title: String,
      thumbnail: String,
      created: String ,
      commentNumber: String,
      author:String,
      score:String,
      permalink:String,
    }
  }
  </script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped lang="scss">
.item {
    div {
        color:white;
    }
}
  </style>
