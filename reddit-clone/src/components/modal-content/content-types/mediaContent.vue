<template>
        <div class="media" v-html="data.media.oembed.html">
    </div>
</template>
      <script>
    
      import axios from 'axios'
      export default {
        name: 'mediaContent',
        props: {
            data: Object
        },
        methods: {
        },
        computed: {
      }
    }
    
    </script>
    
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../../../scss/_variables.scss";

  .media {
          margin-top: 70px;
          margin-bottom: 70px;
      }

</style>