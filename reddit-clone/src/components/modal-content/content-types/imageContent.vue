<template>
    <div>
        <img class="image" v-bind:src="data.url">
    </div>
</template>
      <script>
    
      import axios from 'axios'
      export default {
        name: 'imageContent',
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

    .image {
            margin: auto;
            margin-top: 80px;
            max-width: 700px;
            max-height: 400px;
            width: auto;
            margin-top: 70px;
            margin-bottom: 70px;
        }

</style>